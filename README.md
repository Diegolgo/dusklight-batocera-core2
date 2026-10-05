# Dusklight para Batocera v39 (Core 2 Duo)

Build de https://github.com/TwilitRealm/dusklight para Batocera v39 (glibc 2.37), compilado
con -march=core2 para CPUs sin AVX. No incluye ROM ni assets del juego.

Commit base de Dusklight:

## Qué se cambió
- dusklight-core2.patch: compatibilidad con libstdc++ de GCC 12 (const_cast en texture.cpp,
  std::ranges::find en lugar de ranges::contains en el randomizer).
- isoc23_shim.c: redirige __isoc23_strtol/strtoul/strtoll/strtoull (glibc 2.38) a las
  versiones normales; la Dawn precompilada las necesita.

## Cómo se compiló (contenedor Debian 12, clang 18, cmake 3.25)
cmake --preset linux-clang-relwithdebinfo \
  -DCMAKE_C_FLAGS="-march=core2" -DCMAKE_CXX_FLAGS="-march=core2" \
  -DCMAKE_EXE_LINKER_FLAGS="-static-libstdc++ -static-libgcc -L/ruta/a/gcc13lib /ruta/a/isoc23_shim.o"
(libstdc++.a tomada de la imagen docker gcc:13)

## Uso en Batocera
Copiar la carpeta "install" a /userdata/roms/ports/ y lanzar:
./dusklight --dvd /ruta/a/TwilightPrincess.rvz
