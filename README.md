# Bladesong Demo — Traducción al Español

> ⚠️ Este proyecto no está afiliado ni respaldado por **SUN AND SERPENT creations** ni **Mythwright**. Todos los derechos del juego pertenecen a sus respectivos dueños. Este repositorio contiene únicamente trabajo creativo original (traducción) y no distribuye assets del juego.

---

## Estado actual

| Componente | Estado |
|------------|--------|
| StringTables | ✅ Traducidos y funcionando |
| Game.locres (interfaz, logros, mensajes) | ✅ Traducido y funcionando |

---

## Requisitos previos

Antes de instalar la traducción necesitas tener el juego instalado y las siguientes herramientas:

| Herramienta | Descarga | Para qué sirve |
|-------------|----------|----------------|
| **repak** | [github.com/trumank/repak/releases](https://github.com/trumank/repak/releases) | Extraer y reempaquetar el `.pak` del juego |

Descarga `repak-x86_64-pc-windows-msvc.zip`, extrae el `.exe` y colócalo en una carpeta accesible, por ejemplo `C:\repak\`.

---

## Instalación

### Paso 1 — Backup del pak original

Antes de modificar nada, **guarda una copia del pak original**. Si algo sale mal puedes restaurarlo.

```powershell
Copy-Item "C:\Program Files (x86)\Steam\steamapps\common\Bladesong Demo\Bladesong\Content\Paks\pakchunk0-Windows.pak" "C:\Users\TU_USUARIO\Desktop\pakchunk0-Windows.pak.backup"
```

### Paso 2 — Extraer el pak

```powershell
& "C:\repak\repak.exe" unpack "C:\Users\TU_USUARIO\Desktop\pakchunk0-Windows.pak.backup" -o "C:\Users\TU_USUARIO\Desktop\PakExtraido"
```

Esto puede tardar varios minutos. Extraerá ~4500 archivos.

### Paso 3 — Copiar el Game.locres traducido

Descarga el archivo `Game.locres` de este repositorio (carpeta `Locres/es/`) y cópialo aquí:

```powershell
Copy-Item "C:\ruta\donde\descargaste\Game.locres" "C:\Users\TU_USUARIO\Desktop\PakExtraido\Bladesong\Content\Localization\Game\en\Game.locres" -Force
```

### Paso 4 — Reempaquetar

```powershell
& "C:\repak\repak.exe" pack "C:\Users\TU_USUARIO\Desktop\PakExtraido" "C:\Users\TU_USUARIO\Desktop\pakchunk0-Windows-ES.pak"
```

### Paso 5 — Reemplazar el pak del juego

```powershell
Copy-Item "C:\Users\TU_USUARIO\Desktop\pakchunk0-Windows-ES.pak" "C:\Program Files (x86)\Steam\steamapps\common\Bladesong Demo\Bladesong\Content\Paks\pakchunk0-Windows.pak" -Force
```

### Paso 6 — Copiar los StringTables

Descarga la carpeta `StringTables/` de este repositorio y copia su contenido directamente a:

```
C:\Program Files (x86)\Steam\steamapps\common\Bladesong Demo\Bladesong\Content\
```

Respetando la estructura de subcarpetas. Estos archivos no requieren reempaquetar nada.

### Paso 7 — Verificar

Abre el juego desde Steam. Los textos deben aparecer en español.

---

## Desinstalar / Restaurar

Si quieres volver al inglés original, restaura el backup:

```powershell
Copy-Item "C:\Users\TU_USUARIO\Desktop\pakchunk0-Windows.pak.backup" "C:\Program Files (x86)\Steam\steamapps\common\Bladesong Demo\Bladesong\Content\Paks\pakchunk0-Windows.pak" -Force
```

Y elimina los StringTables que copiaste manualmente.

---

## Estructura del repositorio

```
/
├── Locres/
│   ├── es/
│   │   └── Game.locres       ← Archivo traducido listo para usar
│   └── Game.json             ← JSON fuente de la traducción (editable)
├── StringTables/             ← Archivos CSV traducidos
└── README.md
```

---

## Cómo funciona — Notas técnicas

El juego usa dos sistemas distintos para los textos:

**StringTables** son archivos CSV que el juego carga directamente desde la carpeta `Content/` sin necesidad de estar empaquetados. Se editan y copian directamente.

**Game.locres** está empaquetado dentro de `pakchunk0-Windows.pak`. A pesar de que el juego usa IoStore (`.ucas/.utoc`), el archivo de localización vive en el `.pak` clásico. Intentar inyectarlo con un pak adicional de override no funciona porque IoStore tiene prioridad — la única solución es reemplazar el pak completo con uno modificado que contenga el `Game.locres` traducido.

---

## Compatibilidad

- **Versión del juego:** Bladesong Demo (build de septiembre 2025)
- **Motor:** Unreal Engine 5.7
- **Plataforma:** PC (Steam) — Windows

> ⚠️ Si el juego recibe una actualización, el pak original cambiará y la traducción dejará de funcionar hasta que se actualice este repositorio.

---

## Contribuir

¿Encontraste un texto sin traducir, un error o quieres mejorar algo?

1. Fork del repositorio
2. Edita el `Game.json` o los StringTables correspondientes
3. Abre un Pull Request describiendo los cambios

---

## Aviso legal

Este proyecto es una traducción no oficial creada por fans, sin fines de lucro. No se distribuyen assets originales del juego — solo los archivos de traducción generados por los colaboradores de este repositorio. El uso de estas instrucciones requiere poseer una copia legítima del juego en Steam.

**Bladesong** es propiedad de © SUN AND SERPENT creations / Mythwright. Todos los derechos reservados.
