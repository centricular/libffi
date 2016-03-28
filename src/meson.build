ffi_sources = [
  'prep_cif.c',
  'types.c',
  'raw_api.c',
  'java_raw_api.c',
  'closures.c',
]

if get_option('debug')
  ffi_sources += ['debug.c']
endif

arch_subdir = host_cpu_family
if host_cpu_family == 'mips'
  ffi_conf.set('TARGET', 'MIPS')
  ffi_sources += ['mips/ffi.c', 'mips/o32.S', 'mips/n32.S']
elif host_cpu_family == 'blackfin'
  ffi_conf.set('TARGET', 'BFIN')
  arch_subdir = 'bfin'
  ffi_sources += ['bfin/ffi.c', 'bfin/sysv.S']
elif host_cpu_family == 'x86_64'
  arch_subdir = 'x86'
  ffi_sources += ['x86/ffi.c']
  if host_system == 'windows'
    ffi_conf.set('TARGET', 'X86_WIN64')
    ffi_sources += ['x86/win64.S']
  elif size_t == 4
    ffi_conf.set('TARGET', 'X86')
    ffi_sources += ['x86/sysv.S', 'x86/win32.S']
  else
    ffi_conf.set('TARGET', 'X86_64')
    ffi_sources += ['x86/ffi64.c', 'x86/unix64.S', 'x86/sysv.S']
  # FIXME: Doesn't support x32
  endif
elif host_cpu_family == 'x86'
  ffi_sources += ['x86/ffi.c']
  # FIXME: This isn't quite correct. Compare with configure.ac
  if host_system == 'freebsd'
    ffi_conf.set('TARGET', 'X86_FREEBSD')
    ffi_sources += ['x86/freebsd.S', 'x86/win32.S']
  elif host_system == 'windows'
    ffi_conf.set('TARGET', 'X86_WIN32')
    ffi_sources += ['x86/win32.S']
  elif host_system == 'darwin'
    ffi_conf.set('TARGET', 'X86_DARWIN')
    ffi_sources += ['x86/darwin.S', 'x86/ffi64.c', 'x86/darwin64']
    if size_t == 4
      ffi_conf.set('TARGET', 'X86_DARWIN32')
      ffi_sources += ['x86/win32.S']
    else
      ffi_conf.set('TARGET', 'X86_DARWIN64')
    endif
  elif host_system == 'solaris'
    if size_t == 4
      ffi_conf.set('TARGET', 'X86')
      ffi_sources += ['x86/sysv.S', 'x86/win32.S']
    else
      ffi_conf.set('TARGET', 'X86_64')
      ffi_sources += ['x86/ffi64.c', 'x86/unix64.c', 'x86/ffi.c', 'x86/sysv.S']
    endif
  elif size_t == 4
    ffi_conf.set('TARGET', 'X86')
    ffi_sources += ['x86/sysv.S', 'x86/win32.S']
  else
    error('Unsupported cpu/host/size_t combination: x86 but size_t != 4')
  # FIXME: Doesn't support x32
  endif
elif host_cpu_family == 'sparc'
  ffi_conf.set('TARGET', 'SPARC')
  ffi_sources += ['sparc/ffi.c', 'sparc/v8.S', 'sparc/v9.S']
elif host_cpu_family == 'alpha'
  ffi_conf.set('TARGET', 'ALPHA')
  ffi_sources += ['alpha/ffi.c', 'alpha/osf.S']
elif host_cpu_family == 'ia64'
  ffi_conf.set('TARGET', 'IA64')
  ffi_sources += ['ia64/ffi.c', 'ia64/unix.S']
elif host_cpu_family == 'm32r'
  ffi_conf.set('TARGET', 'M32R')
  ffi_sources += ['m32r/ffi.c', 'm32r/sysv.S']
elif host_cpu_family == 'm68k'
  ffi_conf.set('TARGET', 'M68K')
  ffi_sources += ['m68k/ffi.c', 'm68k/sysv.S']
elif host_cpu_family == 'm88k'
  ffi_conf.set('TARGET', 'M88K')
  ffi_sources += ['m88k/ffi.c', 'm88k/obsd.S']
elif host_cpu_family == 'moxie'
  ffi_conf.set('TARGET', 'MOXIE')
  ffi_sources += ['moxie/ffi.c', 'moxie/eabi.S']
elif host_cpu_family == 'microblaze'
  ffi_conf.set('TARGET', 'MICROBLAZE')
  ffi_sources += ['microblaze/ffi.c', 'microblaze/sysv.S']
elif host_cpu_family == 'nios2'
  ffi_conf.set('TARGET', 'NIOS2')
  ffi_sources += ['nios2/ffi.c', 'nios2/sysv.S']
elif host_cpu_family == 'or1k'
  ffi_conf.set('TARGET', 'OR1K')
  ffi_sources += ['or1k/ffi.c', 'or1k/sysv.S']
elif host_cpu_family == 'powerpc'
  if host_system == 'aix'
    ffi_conf.set('TARGET', 'POWERPC_AIX')
    ffi_sources += ['powerpc/ffi_darwin.c', 'powerpc/aix.S',
      'powerpc/aix_closure.S']
  elif host_system == 'freebsd'
    ffi_conf.set('TARGET', 'POWERPC_FREEBSD')
    ffi_sources += ['powerpc/ffi.c', 'powerpc/ffi_sysv.c', 'powerpc/sysv.S',
      'powerpc/ppc_closure.S']
  elif host_system == 'darwin'
    ffi_conf.set('TARGET', 'POWERPC_DARWIN')
    ffi_sources += ['powerpc/ffi_darwin.c', 'powerpc/darwin.S',
      'powerpc/darwin_closure.S']
  else
    ffi_conf.set('TARGET', 'POWERPC')
    ffi_sources += ['powerpc/ffi.c', 'powerpc/ffi_sysv.c',
      'powerpc/ffi_linux64.c', 'powerpc/sysv.S', 'powerpc/ppc_closure.S',
      'powerpc/linux64.S', 'powerpc/linux64_closure.S']
  endif
elif host_cpu_family == 'aarch64'
  ffi_conf.set('TARGET', 'AARCH64')
  ffi_sources += ['aarch64/ffi.c', 'aarch64/sysv.S']
elif host_cpu_family == 'arc'
  ffi_conf.set('TARGET', 'ARC')
  ffi_sources += ['arc/ffi.c', 'arc/arcompact.S']
elif host_cpu_family == 'arm'
  ffi_conf.set('TARGET', 'ARM')
  ffi_sources += ['arm/ffi.c', 'arm/sysv.S']
  if host_system == 'darwin'
    ffi_sources += ['arm/trampoline.S']
  endif
elif host_cpu_family == 'avr32'
  ffi_conf.set('TARGET', 'AVR32')
  ffi_sources += ['avr32/ffi.c', 'avr32/sysv.S']
elif host_cpu_family == 'cris'
  ffi_conf.set('TARGET', 'LIBFFI_CRIS')
  ffi_sources += ['cris/ffi.c', 'cris/sysv.S']
elif host_cpu_family == 'frv'
  ffi_conf.set('TARGET', 'FRV')
  ffi_sources += ['frv/ffi.c', 'frv/eabi.S']
elif host_cpu_family == 's390'
  ffi_conf.set('TARGET', 'S390')
  ffi_sources += ['s390/ffi.c', 's390/sysv.S']
elif host_cpu_family == 'sh'
  ffi_conf.set('TARGET', 'SH')
  ffi_sources += ['sh/ffi.c', 'sh/sysv.S']
elif host_cpu_family == 'sh64'
  ffi_conf.set('TARGET', 'SH64')
  ffi_sources += ['sh64/ffi.c', 'sh64/sysv.S']
elif host_cpu_family == 'pa'
  if host_system == 'hpux'
    ffi_conf.set('TARGET', 'PA_HPUX')
    ffi_sources += ['pa/ffi.c', 'pa/hpux32.S']
  else
    ffi_conf.set('TARGET', 'PA_LINUX')
    ffi_sources += ['pa/ffi.c', 'pa/linux.S']
  endif
elif host_cpu_family == 'tile'
  ffi_conf.set('TARGET', 'TILE')
  ffi_sources += ['tile/ffi.c', 'tile/tile.S']
elif host_cpu_family == 'xtensa'
  ffi_conf.set('TARGET', 'XTENSA')
  ffi_sources += ['xtensa/ffi.c', 'xtensa/sysv.S']
elif host_cpu_family == 'metag'
  ffi_conf.set('TARGET', 'METAG')
  ffi_sources += ['metag/ffi.c', 'metag/sysv.S']
elif host_cpu_family == 'vax'
  ffi_conf.set('TARGET', 'VAX')
  ffi_sources += ['vax/ffi.c', 'vax/elfbsd.S']
else
  error('Unsupported cpu family: ' + host_cpu_family)
endif

ffi_lib = library('ffi', ffi_sources,
  include_directories : ffiinc,
  # Taken from the libtool-version file
  # current.revision.age
  version : '6.0.4',
  # current
  soversion : '6',
  install : true)
