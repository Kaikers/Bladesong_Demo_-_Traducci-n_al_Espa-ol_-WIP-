# Bladesong Demo — Traducción al Español (WIP)

> Estado: **En progreso** — StringTables traducidos y funcionando. Game.locres traducido pero pendiente de inyección.

---

## Contexto del proyecto

Traducción no oficial de **Bladesong Demo** al español.

- **Motor:** Unreal Engine 5.7
- **Plataforma:** PC (Steam)
- **Archivos del juego:**
  ```
  Bladesong/Content/Paks/
  ├── global.ucas
  ├── global.utoc
  ├── pakchunk0-Windows.pak
  ├── pakchunk0-Windows.ucas
  └── pakchunk0-Windows.utoc
  ```

---

## Sistemas de texto del juego

El juego usa dos sistemas distintos para los textos:

### 1. StringTables ✅ Resuelto

- Existen como archivos CSV fuera del `.pak`
- Se editan directamente y el juego los carga sin modificar ningún pak
- **Estado:** Traducidos y funcionando correctamente

### 2. Game.locres ❌ Pendiente de inyección

- Ubicación dentro del pak: `Bladesong/Content/Localization/Game/en/Game.locres`
- Contiene textos de interfaz, logros, mensajes, nombres, etc.
- Según FModel, está dentro de `pakchunk0-Windows.pak`
- El juego también usa IoStore (`.ucas/.utoc`), lo que complica el override

---

## Trabajo realizado

### Extracción
- `Game.locres` extraído con **FModel**

### Conversión y traducción
- Convertido a JSON con **UnrealLocres** / **LocResUtility**
- JSON traducido manualmente
- Recompilado a `Game.locres`
- Validado en FModel: los textos traducidos aparecen correctamente

### Intentos de inyección (todos fallidos)

| Método | Resultado |
|--------|-----------|
| Pak adicional `pakchunk99` con **repak** | El juego lo ignora |
| Pak adicional con **UnrealPak UE5.7** | Solo genera `.pak`, sin `.utoc/.ucas` |
| **ZenTools** `ExtractPackages` | Error: `Too new TOC header version` en `global.utoc` — no soporta UE5.7 |
| Archivo suelto en `Content/Localization/Game/en/` | El juego lo ignora completamente |
| Archivo suelto + parámetro `-NoIoStore` | El juego lo sigue ignorando |
| Reemplazar `pakchunk0-Windows.pak` con traducción antigua | `LowLevelFatalError: Could not load section 29 (of 438) of the global shadermap` — versión incompatible |

---

## Análisis del problema

El juego usa el sistema **IoStore** de UE5 (`.ucas/.utoc`). Para que un pak adicional sea reconocido, aparentemente necesita el trío completo:

```
pakchunk99-Windows.pak
pakchunk99-Windows.utoc
pakchunk99-Windows.ucas
```

Ninguna herramienta pública disponible actualmente soporta la generación de IoStore para **UE5.7**:

- **repak** — no genera `.utoc/.ucas`
- **UnrealPak** standalone — no genera IoStore sin proyecto configurado
- **ZenTools** — falla con `Too new TOC header version`

---

## Herramientas utilizadas

| Herramienta | Uso | Resultado |
|-------------|-----|-----------|
| FModel | Inspección y extracción de assets | ✅ Funciona |
| UnrealLocres / LocResUtility | Conversión `.locres` ↔ JSON | ✅ Funciona |
| repak | Empaquetado `.pak` | ⚠️ Solo `.pak`, sin IoStore |
| UnrealPak UE5.7 | Empaquetado `.pak` | ⚠️ Solo `.pak`, sin IoStore |
| ZenTools | Extracción/empaquetado IoStore | ❌ No soporta UE5.7 |

---

## Problema actual

> **¿Cómo crear un pak de override válido (`.pak + .utoc + .ucas`) para UE5.7 que contenga únicamente el `Game.locres` modificado?**

Si tienes experiencia con modding en UE5.7 o conoces una herramienta que soporte IoStore en esta versión, cualquier aporte es bienvenido.

---

## Estructura del repositorio

```
/
├── StringTables/       ← Archivos CSV traducidos (funcionando)
├── Locres/
│   ├── Game.locres     ← Archivo traducido (pendiente de inyección)
│   └── Game.json       ← JSON fuente de la traducción
└── README.md
```

---

## Contribuir

1. Fork del repositorio
2. Crea una rama: `git checkout -b mi-aporte`
3. Haz tus cambios y commitea: `git commit -m "descripción"`
4. Abre un Pull Request

---

## Créditos

- Traducción: en progreso
- Herramientas: FModel, UnrealLocres, repak, ZenTools
