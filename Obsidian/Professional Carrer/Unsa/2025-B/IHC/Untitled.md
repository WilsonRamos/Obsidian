Crear una pagina web (lo mas natural posible):
Low fi Prototype
	para vender video Juego
	Proyecto del curso

Protitipos de bajo nivel -> bosquejos en papel

seguir principios de IHC
satisfaccion de usuario

Desarrolla un **proyecto web en React** (precioso, ligero y fácil de compilar y ejecutar en `localhost`) cuyo objetivo sea **presentar y vender** el videojuego de realidad virtual **The Phantom Heist** cumpliendo los principios de diseño mencionados arriba (justifica ). La web debe ser una landing/product page minimalista y profesional que explique el juego, muestre el prototipo de baja fidelidad y presente al equipo

## Objetivo principal

- Landing page clara y enfocada en convertir visitantes en interesados (información, demo/prototipo, contacto).
    
- Diseño ligero y no sobrecargado; navegación sencilla y accesible.
    

## Requerimientos funcionales

1. **Secciones visibles y separadas (responsive)**:
    
    - Hero (título, tagline, CTA: “Ver prototipo” / “Contacto”).
        
    - Resumen del juego con las **4 fases** (cada fase: nombre, descripción breve + un ícono).
        
    - Página/Sección de **Gameplay interactivo (demo/prototipo de bajo nivel)** — puede ser un iframe, gif, o escena WebGL/A-Frame/Three.js básica que muestre el concepto: planos interactivos, timeline horario, ejemplo de hackeo progresivo y corte de cables (simulado).
        
    - Sección del **equipo (4 personas)** con foto/avatar, rol sugerido y breve bio.
        
    - Sección de características técnicas (plataformas soportadas, requisitos mínimos estimados).
        
    - Footer con contacto, enlaces legales y CTA secundario.
        
2. **Describir las 4 fases** (tanto en la landing como en una página de detalle):
    
    - **Fase 1 – Recon & Control (Hacker / Soporte)**: planos arquitectónicos interactivos (capas), mapa horario dinámico, localización de sensores, hackeo progresivo (desbloqueo de subsistemas con coste temporal/recursos).
        
    - **Fase 2 – Infiltración (Jugador VR)**: sistema de identidad/disfraz verificado por NPCs, navegación sigilosa, evitar cámaras, interacción física (abrir puertas, mover objetos).
        
    - **Fase 3 – Bóveda (Equipo coordinado)**: puzzle de circuitos; hacker da pistas desde su interfaz y jugador VR usa tijeras virtuales para cortar el cable correcto.
        
    - **Fase 4 – Escape bajo presión**: alarma/countdown (15–20 min ejemplo), oleadas de seguridad, cierres dinámicos de rutas, objetivo: salir con evidencia/objeto antes del punto de no retorno.
        
3. **Principios de diseño de interacción** (implementar y justificar visualmente):
    
    - Claridad visual y jerarquía (priorizar tareas).
        
    - Retroalimentación inmediata para interacciones (micro-animaciones, estados).
        
    - Accesibilidad (contraste, tamaño de fuente, navegación por teclado).
        
    - Minimizar carga cognitiva (poco texto, iconografía clara, pasos guiados).
        
4. **Prototipo**:
    
    - Incluir un prototipo de baja fidelidad dentro de la web (imagen/gif/iframe o escena WebGL simple).
        
    - Si se usa WebGL/A-Frame/Three.js, que sea **simple**: ejemplo: un plano 2D con capas que puedan alternarse y un timeline animado.
        
5. **Equipo (4 personas)**: mostrar roles sugeridos (ejemplo):
    
    - Productor / Product Manager — visión general y roadmap.
        
    - Lead Developer (React / Backend / Networking).
        
    - UX / UI Designer + Prototipado VR.
        
    - Artista 3D / VR Interaction Designer.  
        (Incluir foto/avatar, rol, 1 línea de responsabilidades).
        

## Requerimientos técnicos y entregables

- **Stack recomendado**: React (Vite), Tailwind CSS, React Router, Three.js o A-Frame (opcional para prototipo), deploy local (`npm run dev`) y build (`npm run build`).
    
- **Entregables**:
    
    1. Código fuente listo para ejecutar en `localhost`.
        
    2. README con instrucciones claras de instalación y comandos (`npm install`, `npm run dev`, `npm run build`).
        
    3. Componentes bien organizados y nombrados (estructura de carpetas sugerida).
        
    4. Página responsive con las secciones indicadas.
        
    5. Prototipo embebido (baja fidelidad).
        
    6. Archivo simple `assets/` con imágenes de ejemplo (mockups).
        
    7. Lista de decisiones de diseño (breve) y criterios de aceptación.


COn respecto a los principios de diseño:

analiza 2 aplicaciones(Duolingo) si se cumple el principio de diseno captura de pantalla y tambien propuesta de mejora como sugieres 

	* definir para que tipos de usuarioson son conicmiento no expertos
	* bajo que dimensiones de usabilidad fue evaluada esa interztas
		* 
En esas misas aplicaciones identifica perpeciones 

5 min para exponer las respuestas


FeedBack:

### Feedback visual con colores (verde para correcto)
Pantalla de resultados detallada (EXP, porcentaje de aciertos, tiempo)
Sonidos de audio para reforzar respuestas
![[Pasted image 20250930212525.png]]
### Affordance (Affordancia)

Los íconos de audio sugieren claramente su función
![[Pasted image 20250930212510.png]]

### Constraints (Restricciones)
![[Pasted image 20250930212448.png]]
Lecciones bloqueadas que solo se desbloquean progresivamente se podria mejorar poniendole conectores ya se no son obvios(esto para affordance)

## MEJORAS : 
Progreso de lección no visible durante ejercicio
![[Pasted image 20250930212130.png]]

![[Pasted image 20250930211321.png]]

**Mejora: Feedback más educativo en errores**
![[Pasted image 20250930211722.png]]

MEJORA : 
![[Pasted image 20250930211824.png]]


Para cada aplicacion se identifican los usuarios obejtivo , las dimensiones de usabilidad aplicadas

como cada interfaz maneja las experctivas perceptuales de los usuarios segun la teoria conginitiva de Jetff 





![[Pasted image 20251002095805.png]]

Limites Configurables 
![[Pasted image 20251002095849.png]]