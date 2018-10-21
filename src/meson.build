ffi_c_sources = [
  'prep_cif.c',
  'types.c',
  'raw_api.c',
  'java_raw_api.c',
  'closures.c',
]

ffi_asm_sources = []

if get_option('ffi-debug')
  ffi_c_sources += ['debug.c']
endif

# We only support x86 on Windows and Linux right now
if host_system == 'windows'
  # No ARM support for Windows in libffi
  arch_subdir = 'x86'
  if size_t == 4
    TARGET = 'X86_WIN32'
    if cc.get_id() == 'msvc'
      ffi_c_sources += ['x86/ffiold-msvc.c']
      ffi_asm_sources += ['x86/win32_msvc.S']
    else
      ffi_c_sources += ['x86/ffi.c']
      ffi_asm_sources += ['x86/sysv.S']
    endif
  else
    TARGET = 'X86_WIN64'
    ffi_c_sources += ['x86/ffiw64.c']
    if cc.get_id() == 'msvc'
      ffi_asm_sources += ['x86/win64_intel.S']
    else
      ffi_asm_sources += ['x86/win64.S']
    endif
  endif
elif host_system == 'linux' and host_cpu_family.startswith('x86')
  arch_subdir = 'x86'
  if size_t == 4
    # FIXME: gnux32 support?
    TARGET = 'X86'
    ffi_c_sources += ['x86/ffi.c']
    ffi_asm_sources += ['x86/sysv.S']
  else
    TARGET = 'X86_64'
    ffi_c_sources += ['x86/ffi64.c', 'x86/ffiw64.c']
    ffi_asm_sources += ['x86/unix64.S', 'x86/win64.S']
  endif
elif host_system == 'linux' and host_cpu_family.startswith('aarch64')
  arch_subdir = 'aarch64'
  TARGET = 'AARCH64'
  ffi_c_sources += ['aarch64/ffi.c']
  ffi_asm_sources += ['aarch64/sysv.S']
else
  error('Unsupported system "@0@" or cpu family "@1@"'.format(host_system, host_cpu_family))
endif

# Used in ffi.h.in to generate ffi-$arch.h
ffi_conf.set('TARGET', TARGET)
ffi_conf.set('VERSION', meson.project_version())

if cc.get_id() == 'msvc'
  # GCC and Clang accept assembly files directly, but MSVC's cl.exe does not.
  # You need to manually pass the files through the pre-processor first and
  # then through the assembler, and then link the objects into the target.
  # FIXME: Add native support in Meson for this.
  cl = find_program('cl')
  ml = find_program('ml', 'ml64')
  ffi_asm_objs = []
  foreach asm_source : ffi_asm_sources
    incflags = ['/I' + join_paths(meson.current_source_dir(), '..'),
                '/I' + join_paths(meson.current_build_dir(), '..'),
                '/I' + join_paths(meson.current_source_dir(), '..', 'include'),
                '/I' + join_paths(meson.current_build_dir(), '..', 'include')]
    preproc_name = asm_source.underscorify() + '.i'
    obj_name = asm_source.underscorify() + '.obj'
    preproc = custom_target(preproc_name,
        input : asm_source,
        output : preproc_name,
        command : [cl, '/nologo', '/EP', '/P', '/Fi@OUTPUT@',
                   '/DTARGET=' + TARGET, '/DFFI_BUILDING',
                   '@INPUT@'] + incflags)
    ffi_asm_objs += custom_target(obj_name,
        input : preproc,
        output : obj_name,
        command : [ml, '/Fo', '@OUTPUT@', '/c', '@INPUT@'])
  endforeach
  ffi_asm_sources = ffi_asm_objs
endif

ffi_lib = library('ffi', ffi_c_sources, ffi_asm_sources,
  c_args : '-DTARGET=' + TARGET,
  include_directories : ffiinc,
  # Taken from the libtool-version file
  # current - age . age . revision
  version : '7.1.0',
  # current - age
  soversion : '7',
  # current + 1
  darwin_versions : '9',
  install : true)

pkgconf = import('pkgconfig')
pkgconf.generate(ffi_lib,
  description : 'Library supporting Foreign Function Interfaces',
  filebase : 'libffi')

ffi_dep = declare_dependency(link_with : ffi_lib,
  include_directories : ffiinc)
