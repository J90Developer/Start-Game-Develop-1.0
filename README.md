# Galactic Dominion RTS

**Galactic Dominion RTS** es un videojuego de estrategia en tiempo real (RTS) de ciencia ficción en 3D, inspirado en clásicos atemporales como *StarCraft*, *Command & Conquer* y *Supreme Commander*.

Construido íntegramente con estándares web modernos (**HTML5, CSS3, JavaScript ES Modules, Three.js, Vite y Vitest**), utiliza modelos geométricos y shaders procedurales sin dependencias de activos externos descargables.

---

## 🌟 Características Principales

- **Renderizado 3D de Alto Rendimiento**:
  - Motor Three.js optimizado con mapas de sombras suaves (`PCFSoftShadowMap`), Tone Mapping `ACESFilmic` y niebla volumétrica.
- **Economía Táctica de 3 Recursos y Cadena Logística Real** ([`src/core/GameState.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/core/GameState.js), [`src/entities/GasGeyser.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/entities/GasGeyser.js)):
  - **1. Cristales Nova (Minerales 💎)**: Recurso primordial para la manufactura de tropas, drones e infraestructura básica.
  - **2. Gas de Plasma (Exótico 🧪)**: Combustible refinado extraído de géiseres geotérmicos para unidades pesadas, blindados y tecnología de asedio.
  - **3. Red Eléctrica (Energía ⚡)**: Generada por el Centro de Mando y *Reactores Solares* (+60⚡). Consumida por vehículos avanzados e instalaciones. Alertas automáticas de sobrecarga ante déficit.
  - **4. Límite de Población (Control de Mando 👥)**: Gestión estricta de suministros ampliable mediante *Repetidores de Suministro* (+12👥) hasta un límite de 200.
  - **Ciclo Autónomo de Recolección y Transporte**:
    - Los Drones Recolectores reciben órdenes con clic derecho sobre cualquier yacimiento o géiser.
    - Se desplazan mediante **navegación A***, extraen recursos durante un tiempo de canalización y cargan un contenedor visual 3D (cristal cian o cápsula de plasma esmeralda).
    - Regresan automáticamente a la bahía de descarga del Centro de Mando, depositan el recurso en el estado real del juego y retornan al yacimiento sin intervención del jugador hasta agotarlo.
  - **Almacenamiento y Depleción**: Los yacimientos de minerales y depósitos de gas tienen reservas finitas que se van agotando visual y lógicamente.
- **Sistema Avanzado de Control Táctico RTS** ([`src/systems/SelectionSystem.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/systems/SelectionSystem.js)):
  - **Selección Individual**: Clic izquierdo sobre cualquier unidad, estructura o recurso.
  - **Selección Múltiple por Rectángulo (Marquee)**: Arrastre de ratón para envolver y seleccionar escuadrones de unidades aliadas en pantalla.
  - **Modificador Shift**: Añade o retira unidades de la selección activa mediante <kbd>Shift</kbd> + <kbd>Clic</kbd> o <kbd>Shift</kbd> + <kbd>Arrastrar</kbd>.
  - **Protección contra Clics Incidentales**: La interacción con paneles del HUD, radar y botones no transfiere órdenes al terreno 3D.
  - **Órdenes Tácticas Clásicas**:
    - **Detenerse (<kbd>S</kbd>)**: Cancela desplazamientos y detiene motores al instante.
    - **Mantener Posición (<kbd>H</kbd>)**: La unidad defiende su enclave sin moverse ni perseguir.
    - **Patrullar (<kbd>P</kbd>)**: Guardia continua de ida y vuelta entre dos puntos tácticos.
  - **Movimiento en Grupo y Formaciones**: Distribución radial/cuadrícula para evitar que múltiples unidades colapsen en una única coordenada.
  - **Fuerza de Separación (Flocking)**: Algoritmo de repulsión entre unidades cercanas para evitar solapamientos y apilamientos.
- **Navegación A* y Evasión Dinámica de Obstáculos** ([`src/navigation/Pathfinder.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/navigation/Pathfinder.js)):
  - Algoritmo A* en 8 direcciones con costes ortogonales ($1.0$) y diagonales ($1.414$).
  - Prevención de corte de esquinas entre montañas y rocas.
  - Búsqueda automática de la celda transitable más cercana ante destinos obstruidos o inaccesibles.
  - Suavizado de trayectoria (*string pulling / line-of-sight*) para evitar movimientos en zigzag.
  - Movimiento gradual sin teletransportes siguiendo waypoints con interpolación angular.
- **Panel de Información Táctica Dual** ([`src/ui/SelectionPanel.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/ui/SelectionPanel.js)):
  - *Vista Individual*: Estadísticas completas (HP, Escudo, Daño, Alcance, Armadura, Velocidad, Facción).
  - *Vista de Grupo Táctico*: Recuento consolidado de unidades, barras acumuladas de vida/escudo y composición de escuadra.
- **Fuerzas Militares y Clases de Unidades Originales** ([`src/entities/Unit.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/entities/Unit.js), [`src/data/unitsData.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/data/unitsData.js)):
  - **1. Trabajador Recolector (`Eclipse`)**: Dron de levitación magnética con toberas pulsantes y brazos articulados para extracción de mineral.
  - **2. Soldado de Infantería (`Vanguard`)**: Tropa de asalto bipedal con ciclo de marcha animado, armadura exoesqueleto y rifle de plasma dual.
  - **3. Explorador Rápido (`Skimmer`)**: Deslizador supersónico con alerones en flecha, inclinación lateral en curvas (*banking roll*) y postcombustor reactivo.
  - **4. Vehículo Blindado (`Titan`)**: Tanque pesado con orugas acorazadas, chasis sloped y torreta giratoria independiente con cañones gemelos.
  - **5. Artillería Pesada (`Nova-Siege`)**: Plataforma balística con patas estabilizadoras, radar giratorio y cañón colosal de asedio con elevación y retroceso.
- **Separación Limpia de Lógica y Renderizado**:
  - Parámetros configurables en [`src/data/unitsData.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/data/unitsData.js): vida, velocidad, armadura, daño, alcance de ataque, visión, costes y tiempos de recarga.
  - Modelado 3D procedural encapsulado en [`src/entities/models/UnitModelFactory.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/entities/models/UnitModelFactory.js).
- **Diferenciación Visual Aliados vs. Enemigos**:
  - **Aliados (`Solaris Vanguard`)**: Acabado en titanio pizarra, luminiscencia cian (`#00f0ff`) y radar en verde/cian.
  - **Enemigos (`Crimson Syndicate`)**: Acabado en acero obsidiana oscuro, luminiscencia carmesí (`#ff2a55`) y radar en rojo de amenaza.
- **Cámara Ortográfica Isométrica RTS Cinematográfica** ([`src/camera/IsometricOrthoCamera.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/camera/IsometricOrthoCamera.js)):
  - Proyección ortográfica pura con el ángulo isométrico clásico RTS (35.264° / 45°).
  - Paneo fluido con teclado mediante teclas <kbd>W</kbd>, <kbd>A</kbd>, <kbd>S</kbd>, <kbd>D</kbd> o <kbd>Flechas</kbd>.
  - Desplazamiento automático al acercar el cursor a los bordes de la pantalla (**Edge Scrolling**).
  - Zoom dinámico con la rueda del ratón con escala de frustum ortográfico.
  - Confinamiento estricto dentro de los límites del sector de batalla.
  - Centrado instantáneo en la base con la tecla <kbd>Espacio</kbd>.
- **Mapa 3D Alienígena & Entorno Táctico** ([`src/map/AlienWorldMap.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/map/AlienWorldMap.js)):
  - Terreno planetario con textura canvas y vetas bioluminiscentes subterráneas.
  - **Cordilleras Montañosas**: elevaciones escarpadas que delimitan cañones y cuellos de botella estratégicos (impenetrables).
  - **Formaciones Rocosas**: cúmulos de agujas de basalto alienígena y peñascos de obsidiana.
  - **Instalaciones Futuristas Procedurales**:
    - *Subestación de Plasma Cuántica* con núcleo giratorio pulsante y arcos de contención.
    - *Torre de Sensores y Vigilancia Orbital* con antena de rastreo de 360°.
    - *Búnker Defensivo de Campo Aegis* con cúpula hemisférica de plasma.
  - **Zonas Designadas de Construcción**: plataformas de expansión con balizas y anillos holográficos.
- **Cuadrícula Espacial de Construcción y Tránsito** ([`src/map/GridSystem.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/map/GridSystem.js)):
  - Sistema de celdas con clasificación binaria y funcional (`PASSABLE`, `BUILDABLE`, `OBSTACLE`, `OCCUPIED`).
  - Cursor de selección holográfico dinámico: resalta en verde cian las zonas aptas para construir y en rojo carmesí los obstáculos y montañas.
  - Alternancia de la cuadrícula con la tecla <kbd>G</kbd> o desde el panel de órdenes.
  - Validación de trayectorias de unidades contra obstáculos y zonas bloqueadas.
- **Iluminación y Sombras Optimizadas** ([`src/environment/LightingSystem.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/environment/LightingSystem.js)):
  - Sol orbital con frustum ortográfico ajustado al mapa para máxima nitidez de sombras (`PCFSoftShadowMap` a 2048x2048) manteniendo 60 FPS estables.
- **HUD de Depuración / Rendimiento en Tiempo Real** ([`src/ui/DebugOverlay.js`](file:///c:/Users/JUANDE/Downloads/Navegador/src/ui/DebugOverlay.js)):
  - Indicador de FPS instantáneos con semáforo cromático de rendimiento.
  - Tiempo de renderizado de frame en milisegundos (`ms`).
  - Coordenadas de posición de cámara en el mundo (`X`, `Z`) y nivel de zoom ortográfico.
  - Estado en vivo de la celda de la cuadrícula bajo el cursor (`Transitable`, `Construible`, `Obstáculo`).
  - Alternable mediante la tecla <kbd>F3</kbd>.
- **Cúpula Cósmica (SkyDome)**:
  - Campo estelar procedural y gigante gaseoso distante con sistema de anillos orbitales.
- **Centro de Mando Solaris (HQ Futurista)**:
  - Estructura modular octogonal con blindaje de aleación de titanio.
  - Reactor de fusión taquiónico en la cima con pulsación lumínica y anillos magnéticos en contrarrotación.
  - Antena parabólica de radar con giro continuo de 360 grados.
  - Bahía de despliegue de unidades y balizas luminosas de señalización.
- **Entidades de Apoyo**:
  - **Yacimientos de Cristales Nova**: formaciones cristalinas translúcidas con brillo emisivo interactivo.
  - **Drones Mineros Eclipse V-1**: unidades móviles de levitación magnética con propulsores antigravedad y desplazamiento sobre el terreno.
- **HUD Táctico Completo**:
  - Barra de telemetría superior: Cristales Nova, Energía de Fusión, Control de Mando y contador de FPS.
  - Radar táctico 2D / Minimapa interactivo con clics para reubicar la cámara.
  - Ficha de telemetría con barras de blindaje y escudos regenerables.
  - Panel de órdenes clásico con botonera interactiva y atajos de teclado (`Q`, `W`, `E`, `R`, `S`).
  - Feed de notificaciones tácticas en tiempo real.
- **Arquitectura Modular Desacoplada**:
  - Núcleo (`core/`), Cámara (`camera/`), Entorno (`environment/`), Entidades (`entities/`), Sistemas (`systems/`) e Interfaz (`ui/`).
  - Comunicación a través de un Bus de Eventos (`EventBus`) pub/sub.
- **Bucle de Simulación a 60 FPS**:
  - `GameLoop` con delta-time protegido contra saltos y acumulación por subpasos estables.

---

## 🕹️ Controles del Juego

| Acción | Control |
|---|---|
| **Desplazar cámara** | Teclas `W`, `A`, `S`, `D` o `Flechas del teclado` |
| **Desplazamiento por bordes (Edge Scroll)** | Acercar el cursor del ratón a menos de 24px de cualquier borde |
| **Zoom Ortográfico In / Out** | `Rueda del ratón` |
| **Centrar en Base** | Tecla `Espacio` |
| **Alternar Cuadrícula de Construcción** | Tecla `G` o botón en panel de órdenes |
| **Alternar HUD de Depuración / FPS** | Tecla `F3` o clic en el indicador |
| **Seleccionar Entidad** | `Click Izquierdo` sobre la entidad |
| **Selección Múltiple por Caja** | Mantener pulsado `Click Izquierdo` y arrastrar sobre las unidades |
| **Añadir / Quitar de Selección** | `Shift` + `Click Izquierdo` o `Shift` + `Arrastrar Caja` |
| **Mover Unidad / Grupo** | `Click Derecho` sobre el terreno (navega con A* y evita obstáculos) |
| **Detener Unidad / Grupo** | Botón en Panel o tecla `S` |
| **Mantener Posición** | Botón en Panel o tecla `H` |
| **Patrullar Zona** | Botón en Panel o tecla `P` |
| **Fabricar Dron Minero** | Botón en Base o tecla `Q` |
| **Reclutar Exosoldado** | Botón en Base o tecla `W` |
| **Construir Deslizador** | Botón en Base o tecla `E` |
| **Construir Tanque Titan** | Botón en Base o tecla `R` |
| **Construir Mortero Nova** | Botón en Base o tecla `T` |
| **Saltar con el Radar** | `Click Izquierdo` dentro del Radar / Minimapa inferior izquierdo |

---

## 🚀 Requisitos e Instalación

### Requisitos Previos
- **Node.js**: v18.0.0 o superior (recomendado v20+)
- **NPM**: v9.0.0 o superior

### 1. Clonar o acceder a la carpeta del proyecto
```bash
cd c:\Users\JUANDE\Downloads\Navegador
```

### 2. Instalar dependencias
```bash
npm install
```

### 3. Ejecutar en Modo Desarrollo (Vite)
```bash
npm run dev
```
Abre tu navegador en: **`http://localhost:3000`**

### 4. Ejecutar la Suite de Pruebas (Vitest)
```bash
npm run test
```

### 5. Compilar para Producción
```bash
npm run build
```
Los archivos optimizados y empaquetados se generarán en la carpeta `dist/`.

---

## 📁 Estructura del Código

```
├── index.html                  # Punto de entrada HTML con el HUD Sci-Fi
├── package.json                # Configuración de dependencias y scripts
├── vite.config.js              # Configuración de Vite
├── vitest.config.js            # Configuración de pruebas unitarias
├── src/
│   ├── main.js                 # Inicialización y cableado de sistemas
│   ├── style.css               # Estilos del HUD táctico y efectos luminosos
│   ├── data/
│   │   └── unitsData.js        # Configuración independiente de estadísticas, costes y facciones
│   ├── core/
│   │   ├── Engine.js           # Three.js: Renderer, Scene, sombras y resize
│   │   ├── GameLoop.js         # Bucle maestro de simulación a 60 FPS
│   │   ├── EventBus.js         # Bus de eventos desacoplado pub/sub
│   │   └── GameState.js        # Estado reactivo del juego y recursos
│   ├── camera/
│   │   └── IsometricOrthoCamera.js # Cámara ortográfica isométrica con edge scroll y límites
│   ├── map/
│   │   ├── AlienWorldMap.js    # Terreno alienígena, montañas, rocas e instalaciones
│   │   └── GridSystem.js       # Cuadrícula de construcción, obstáculos y transitabilidad
│   ├── navigation/
│   │   └── Pathfinder.js       # Algoritmo A* en 8 direcciones, suavizado y evasión de obstáculos
│   ├── environment/
│   │   ├── LightingSystem.js   # Sol orbital, luz hemisférica y sombras ortográficas optimizadas
│   │   └── SkyDome.js          # Cúpula cósmica con campo de estrellas y planeta
│   ├── entities/
│   │   ├── Entity.js           # Clase base con anillo de selección y atributos
│   │   ├── Unit.js             # Clase de unidad militar: lógica de movimiento, daño y combate
│   │   ├── CommandCenter.js    # Centro de mando procedural detallado
│   │   ├── HarvesterUnit.js    # Dron minero de levitación magnética
│   │   ├── MineralCluster.js   # Yacimientos de cristales Nova reflectantes
│   │   └── models/
│   │       └── UnitModelFactory.js # Factoría procedural 3D de modelos para las 5 unidades
│   ├── systems/
│   │   ├── SelectionSystem.js  # Raycasting para click, hover y validación de obstáculos
│   │   ├── ResourceSystem.js   # Ticks de recursos, ingresos y validación
│   │   └── OrderSystem.js      # Ejecución de comandos de base y unidades
│   └── ui/
│       ├── HUD.js              # Telemetría de recursos superior
│       ├── CommandCard.js      # Panel de órdenes clásico con botonera
│       ├── SelectionPanel.js   # Ficha técnica de la entidad seleccionada
│       ├── MiniRadar.js        # Radar táctico 2D con vista en tiempo real
│       ├── Notifications.js    # Alertas y notificaciones tácticas
│       └── DebugOverlay.js     # Telemetría en vivo: FPS, frame time, posición y celda
└── tests/
    ├── EventBus.test.js        # Pruebas unitarias del bus de eventos
    ├── GameState.test.js       # Pruebas de recursos, selección y entidades
    ├── GridSystem.test.js      # Pruebas de cuadrícula espacial y detección de obstáculos
    ├── HarvestingEconomy.test.js # Pruebas del ciclo autónomo de recolección, transporte y almacén
    ├── Pathfinder.test.js      # Pruebas de navegación A*, desvío de obstáculos y rutas inválidas
    ├── ResourceSystem.test.js  # Pruebas de economía y deducción de costes
    ├── RTSControl.test.js      # Pruebas de selección múltiple, Shift, movimiento gradual y separación
    └── Unit.test.js            # Pruebas de estadísticas, combate y equipos de unidades
```

---

## 🛠️ Tecnologías Empleadas

- **Three.js** (v0.170.0): Biblioteca WebGL para gráficos 3D acelerados por hardware.
- **Vite** (v5.4.10): Servidor de desarrollo con Hot Module Replacement (HMR) ultrarrápido y bundler Rollup.
- **Vitest** (v2.1.4): Framework de pruebas unitarias nativo ESM de alta velocidad.
- **CSS3 Moderno**: Flexbox, CSS Grid, Glassmorphism y animaciones de keyframes para la interfaz Sci-Fi.
