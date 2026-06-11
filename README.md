# Simulador DEM — Molino SAG

Simulador interactivo por Método de Elementos Discretos (DEM) de la sección
transversal de un molino SAG de 8 m de diámetro. Pensado como herramienta
visual para explicar los fenómenos de molienda: cascada, catarata,
centrifugado, efecto de los lifters, hombro y pie de carga, y curva de
potencia.

**Un solo archivo, cero dependencias, cero build.** Todo (física, render, UI)
vive en `index.html`.

## Cómo ejecutarlo

Cualquiera de estas opciones:

```bash
# 1. Abrir directamente (doble clic también funciona)
xdg-open index.html

# 2. Servidor local
python3 -m http.server 8000   # → http://localhost:8000
```

## Despliegue

Por ser un archivo estático sin dependencias, se despliega en segundos:

| Plataforma | Cómo |
|---|---|
| **GitHub Pages** | Subir el repo → Settings → Pages → rama main |
| **Netlify / Vercel** | Arrastrar la carpeta al dashboard |
| **Intranet / SharePoint** | Copiar `index.html` a cualquier servidor de archivos |
| **Sin red** | Enviar el archivo por correo; se abre offline en cualquier navegador |

## Física implementada

- **Contacto resorte-amortiguador lineal** (kn = 3×10⁶ N/m) con amortiguamiento
  derivado del coeficiente de restitución.
- **Fricción de Coulomb** (viscosa tangencial con tope μ·Fn).
- **Integración semi-implícita de Euler** con 10 sub-pasos por frame
  (Δt ≈ 1.7 ms), en tiempo real.
- **Spatial hashing** para búsqueda de vecinos O(N).
- **Lifters** modelados como cápsulas radiales que rotan con el casco.
- **Desgaste de lifters** tipo Archard (abrasión por fricción + impactos,
  las bolas de acero desgastan más), acelerable hasta ×500 para verlo en
  ~1–2 minutos. Con el perfil gastado el hombro cae y la catarata se acorta,
  pero la potencia no baja: molienda peor al mismo costo energético.
- Dos especies: mineral (ρ = 2700 kg/m³) y bolas de acero (ρ = 7800 kg/m³, ~1.45× el radio).
- Unidades SI reales; las partículas están a escala ~10× para que la
  simulación corra a 60 fps en el navegador (hasta ~2600 partículas).

## Variables interactivas

- Velocidad (% de la crítica Nc = 42.3/√D ≈ 15 rpm), en vivo
- Nivel de llenado J (10–45%)
- Carga de bolas (0–40% de la carga)
- Tamaño de partícula
- Número y altura de lifters, en vivo
- Desgaste acelerado del revestimiento (×0–500) con botón de relining
- Fricción μ y restitución e, en vivo

## Salidas para explicar fenómenos

- **Badge de régimen** con explicación del fenómeno actual
- **Potencia consumida** (P = τ·ω desde el torque de reacción sobre el casco) — muestra el máximo ~80% Nc y la caída al centrifugar
- **Espectro de energía de impactos** (histograma log) — distingue abrasión de impacto
- **Hombro y pie de carga** detectados automáticamente
- **Perfil de desgaste por lifter** (barras de altura restante) con nota didáctica según el nivel
- Modos de color: tipo, velocidad, energía de impacto reciente
- Trayectorias y vectores de velocidad

## Presets

`Deslizamiento` (38% Nc) · `Cascada` (62%) · `Catarata` (80%) · `Centrifugado` (115%)
