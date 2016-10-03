ffi_c_sources = [
  'prep_cif.c',
  'types.c',
  'raw_api.c',
  'java_raw_api.c',
  'closures.c',
]

ffi_asm_sources = []

if get_option('debug')
  ffi_c_sources += ['debug.c']
endif

arch_subdir = host_cpu_family
if host_cpu_family == 'mips'
  ffi_conf.set('TARGET', 'MIPS')
  ffi_c_sources += ['mips/ffi.c']
  ffi_asm_sources += ['mips/o32.S', 'mips/n32.S']
elif host_cpu_family == 'blackfin'
  ffi_conf.set('TARGET', 'BFIN')
  arch_subdir = 'bfin'
  ffi_c_sources += ['bfin/ffi.c']
  ffi_asm_sources += ['bfin/sysv.S']
elif host_cpu_family == 'x86_64'
  arch_subdir = 'x86'
  ffi_c_sources += ['x86/ffi.c']
  if host_system == 'windows'
    ffi_conf.set('TARGET', 'X86_WIN64')
    ffi_asm_sources += ['x86/win64.S']
  elif host_system == 'darwin'
    ffi_conf.set('TARGET', 'X86_DARWIN')
    ffi_c_sources += ['x86/ffi64.c']
    ffi_asm_sources += ['x86/darwin.S', 'x86/darwin64.S']
  elif size_t == 4
    ffi_conf.set('TARGET', 'X86')
    ffi_asm_sources += ['x86/sysv.S', 'x86/win32.S']
  else
    ffi_conf.set('TARGET', 'X86_64')
    ffi_c_sources += ['x86/ffi64.c']
    ffi_asm_sources += ['x86/unix64.S', 'x86/sysv.S']
  # FIXME: Doesn't support x32
  endif
elif host_cpu_family == 'x86'
  ffi_c_sources += ['x86/ffi.c']
  # FIXME: This isn't quite correct. Compare with configure.ac
  if host_system == 'freebsd'
    ffi_conf.set('TARGET', 'X86_FREEBSD')
    ffi_asm_sources += ['x86/freebsd.S', 'x86/win32.S']
  elif host_system == 'windows'
    ffi_conf.set('TARGET', 'X86_WIN32')
    ffi_asm_sources += ['x86/win32.S']
  elif host_system == 'darwin'
    ffi_conf.set('TARGET', 'X86_DARWIN')
    ffi_c_sources += ['x86/ffi64.c']
    ffi_asm_sources += ['x86/darwin.S', 'x86/darwin64.S']
    if size_t == 4
      ffi_conf.set('TARGET', 'X86_DARWIN32')
      ffi_asm_sources += ['x86/win32.S']
    else
      ffi_conf.set('TARGET', 'X86_DARWIN64')
    endif
  elif host_system == 'solaris'
    if size_t == 4
      ffi_conf.set('TARGET', 'X86')
      ffi_asm_sources += ['x86/sysv.S', 'x86/win32.S']
    else
      ffi_conf.set('TARGET', 'X86_64')
      ffi_c_sources += ['x86/ffi64.c', 'x86/unix64.c', 'x86/ffi.c']
  ffi_asm_sources += ['x86/sysv.S']
    endif
  elif size_t == 4
    ffi_conf.set('TARGET', 'X86')
    ffi_asm_sources += ['x86/sysv.S', 'x86/win32.S']
  else
    error('Unsupported cpu/host/size_t combination: x86 but size_t != 4')
  # FIXME: Doesn't support x32
  endif
elif host_cpu_family == 'sparc'
  ffi_conf.set('TARGET', 'SPARC')
  ffi_c_sources += ['sparc/ffi.c']
  ffi_asm_sources += ['sparc/v8.S', 'sparc/v9.S']
elif host_cpu_family == 'alpha'
  ffi_conf.set('TARGET', 'ALPHA')
  ffi_c_sources += ['alpha/ffi.c']
  ffi_asm_sources += ['alpha/osf.S']
elif host_cpu_family == 'ia64'
  ffi_conf.set('TARGET', 'IA64')
  ffi_c_sources += ['ia64/ffi.c']
  ffi_asm_sources += ['ia64/unix.S']
elif host_cpu_family == 'm32r'
  ffi_conf.set('TARGET', 'M32R')
  ffi_c_sources += ['m32r/ffi.c']
  ffi_asm_sources += ['m32r/sysv.S']
elif host_cpu_family == 'm68k'
  ffi_conf.set('TARGET', 'M68K')
  ffi_c_sources += ['m68k/ffi.c']
  ffi_asm_sources += ['m68k/sysv.S']
elif host_cpu_family == 'm88k'
  ffi_conf.set('TARGET', 'M88K')
  ffi_c_sources += ['m88k/ffi.c']
  ffi_asm_sources += ['m88k/obsd.S']
elif host_cpu_family == 'moxie'
  ffi_conf.set('TARGET', 'MOXIE')
  ffi_c_sources += ['moxie/ffi.c']
  ffi_asm_sources += ['moxie/eabi.S']
elif host_cpu_family == 'microblaze'
  ffi_conf.set('TARGET', 'MICROBLAZE')
  ffi_c_sources += ['microblaze/ffi.c']
  ffi_asm_sources += ['microblaze/sysv.S']
elif host_cpu_family == 'nios2'
  ffi_conf.set('TARGET', 'NIOS2')
  ffi_c_sources += ['nios2/ffi.c']
  ffi_asm_sources += ['nios2/sysv.S']
elif host_cpu_family == 'or1k'
  ffi_conf.set('TARGET', 'OR1K')
  ffi_c_sources += ['or1k/ffi.c']
  ffi_asm_sources += ['or1k/sysv.S']
elif host_cpu_family == 'powerpc'
  if host_system == 'aix'
    ffi_conf.set('TARGET', 'POWERPC_AIX')
    ffi_c_sources += ['powerpc/ffi_darwin.c']
    ffi_asm_sources += ['powerpc/aix.S', 'powerpc/aix_closure.S']
  elif host_system == 'freebsd'
    ffi_conf.set('TARGET', 'POWERPC_FREEBSD')
    ffi_c_sources += ['powerpc/ffi.c', 'powerpc/ffi_sysv.c']
    ffi_asm_sources += ['powerpc/sysv.S', 'powerpc/ppc_closure.S']
  elif host_system == 'darwin'
    ffi_conf.set('TARGET', 'POWERPC_DARWIN')
    ffi_c_sources += ['powerpc/ffi_darwin.c']
    ffi_asm_sources += ['powerpc/darwin.S', 'powerpc/darwin_closure.S']
  else
    ffi_conf.set('TARGET', 'POWERPC')
    ffi_c_sources += ['powerpc/ffi.c', 'powerpc/ffi_sysv.c', 'powerpc/ffi_linux64.c']
    ffi_asm_sources += ['powerpc/sysv.S', 'powerpc/ppc_closure.S', 'powerpc/linux64.S', 'powerpc/linux64_closure.S']
  endif
elif host_cpu_family == 'aarch64'
  ffi_conf.set('TARGET', 'AARCH64')
  ffi_c_sources += ['aarch64/ffi.c']
  ffi_asm_sources += ['aarch64/sysv.S']
elif host_cpu_family == 'arc'
  ffi_conf.set('TARGET', 'ARC')
  ffi_c_sources += ['arc/ffi.c']
  ffi_asm_sources += ['arc/arcompact.S']
elif host_cpu_family == 'arm'
  ffi_conf.set('TARGET', 'ARM')
  ffi_c_sources += ['arm/ffi.c']
  ffi_asm_sources += ['arm/sysv.S']
  if host_system == 'darwin'
    ffi_asm_sources += ['arm/trampoline.S']
  endif
elif host_cpu_family == 'avr32'
  ffi_conf.set('TARGET', 'AVR32')
  ffi_c_sources += ['avr32/ffi.c']
  ffi_asm_sources += ['avr32/sysv.S']
elif host_cpu_family == 'cris'
  ffi_conf.set('TARGET', 'LIBFFI_CRIS')
  ffi_c_sources += ['cris/ffi.c']
  ffi_asm_sources += ['cris/sysv.S']
elif host_cpu_family == 'frv'
  ffi_conf.set('TARGET', 'FRV')
  ffi_c_sources += ['frv/ffi.c']
  ffi_asm_sources += ['frv/eabi.S']
elif host_cpu_family == 's390'
  ffi_conf.set('TARGET', 'S390')
  ffi_c_sources += ['s390/ffi.c']
  ffi_asm_sources += ['s390/sysv.S']
elif host_cpu_family == 'sh'
  ffi_conf.set('TARGET', 'SH')
  ffi_c_sources += ['sh/ffi.c']
  ffi_asm_sources += ['sh/sysv.S']
elif host_cpu_family == 'sh64'
  ffi_conf.set('TARGET', 'SH64')
  ffi_c_sources += ['sh64/ffi.c']
  ffi_asm_sources += ['sh64/sysv.S']
elif host_cpu_family == 'pa'
  if host_system == 'hpux'
    ffi_conf.set('TARGET', 'PA_HPUX')
    ffi_c_sources += ['pa/ffi.c']
  ffi_asm_sources += ['pa/hpux32.S']
  else
    ffi_conf.set('TARGET', 'PA_LINUX')
    ffi_c_sources += ['pa/ffi.c']
  ffi_asm_sources += ['pa/linux.S']
  endif
elif host_cpu_family == 'tile'
  ffi_conf.set('TARGET', 'TILE')
  ffi_c_sources += ['tile/ffi.c']
  ffi_asm_sources += ['tile/tile.S']
elif host_cpu_family == 'xtensa'
  ffi_conf.set('TARGET', 'XTENSA')
  ffi_c_sources += ['xtensa/ffi.c']
  ffi_asm_sources += ['xtensa/sysv.S']
elif host_cpu_family == 'metag'
  ffi_conf.set('TARGET', 'METAG')
  ffi_c_sources += ['metag/ffi.c']
  ffi_asm_sources += ['metag/sysv.S']
elif host_cpu_family == 'vax'
  ffi_conf.set('TARGET', 'VAX')
  ffi_c_sources += ['vax/ffi.c']
  ffi_asm_sources += ['vax/elfbsd.S']
else
  error('Unsupported cpu family: ' + host_cpu_family)
endif

if cc.get_id() == 'msvc'
  # FIXME: Meson doesn't have built-in support for compiling assembly files.
  # Use the msvcc.sh script to compile the ASM sources.
  #
  # GCC and Clang accept assembly files directly, but MSVC's cl.exe does not.
  # You need to manually pass the files through the pre-processor first and
  # then through the assembler. I tried a whole bunch of things including
  # generators for both (didn't work because generators can't take generator
  # output as input), custom_target() for both (didn't work because the format
  # required is very specific, custom_target is processed during configure,
  # etc). Finally, I gave up and did this.
  args = ['-c', '-I.', '-Iinclude']
  if host_cpu_family == 'x86_64'
    args += ['-m64'] # msvcc.sh checks this gcc flag to switch to 64-bit mode
  endif
  compiler = generator(msvcc,
      output : '@BASENAME@.obj',
      arguments : ['@INPUT@'] + args + ['-o', '@OUTPUT0@'])
  ffi_asm_sources = compiler.process(ffi_asm_sources)
endif

ffi_lib = library('ffi', ffi_c_sources, ffi_asm_sources,
  include_directories : ffiinc,
  # Taken from the libtool-version file
  # current.revision.age
  version : '6.0.4',
  # current
  soversion : '6',
  install : true)
