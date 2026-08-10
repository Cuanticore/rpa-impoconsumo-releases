# RPA Impoconsumo — canal de distribución

Este repositorio **no tiene código**. Es el canal por el que las estaciones de
ILC reciben las actualizaciones de RPA Impoconsumo.

Los paquetes se publican como **assets de cada Release**, no como archivos del
repositorio: pesan más de 400 MB y GitHub rechaza archivos de más de 100 MB.

## Por qué es público

`CuanticoLauncher` consulta este repositorio **sin credenciales**. Si fuera
privado habría que meter un token dentro de cada instalador, y cualquiera que
abra el `.exe` lo encontraría: sería peor que no tener nada.

Que sea público no lo hace inseguro. La protección no está en esconder la URL
sino en la firma: cada release trae `releases.win.json` junto con
`releases.win.json.signature`, una firma **Ed25519** que el launcher verifica
contra la clave pública que lleva grabada. Sin la clave privada —que no está en
ningún repositorio— nadie puede publicar algo que las estaciones acepten.

## Cómo se publica

Desde el repositorio de la aplicación:

```powershell
$env:GITHUB_TOKEN = "<token con permiso de escritura>"
.\build_windows.ps1 -Publicar
```

## Reglas

- **No borres Releases anteriores.** Sus paquetes son lo que permite calcular
  las actualizaciones diferenciales. Sin ellos, cada actualización pasa de ~1 MB
  a más de 400 MB.
- **No subas paquetes a mano.** Un Release sin `releases.win.json.signature`
  deja a todas las estaciones sin poder actualizarse: el launcher rechaza un
  feed sin firma.
