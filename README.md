# ¡IA aprendiendo a jugar Snake! 👾🐍
Una red neuronal que aprende a jugar al juego de snake.

Desarrollado con [Rust](https://www.rust-lang.org/) y el motor de juegos [Macroquad](https://github.com/not-fl3/macroquad).

# Explicación y Video Timelapse
[![TikTok Video](https://img.icons8.com/ios-filled/50/000000/tiktok.png)](https://www.tiktok.com/@antonio_richaud/video/7385358589981887750?is_from_webapp=1&sender_device=pc&web_id=7353830675621348869)

# Controles
- `Tab` - Habilitar/Deshabilitar visualización
- `Space` - Reducir la velocidad de la simulación

# Glosario
- **Neuro-evolución**: Un subcampo de la inteligencia artificial y la computación evolutiva que utiliza algoritmos evolutivos para desarrollar redes neuronales artificiales.
- **Algoritmo Genético (GA)**: Un algoritmo de optimización inspirado en el proceso de selección natural y genética, utilizado para evolucionar poblaciones de soluciones a problemas de optimización.
- **Población**: Un conjunto de individuos (redes neuronales) que son sujetos a evolución en un algoritmo de neuro-evolución.
- **Modelo de Islas**: Una técnica en neuro-evolución donde múltiples poblaciones separadas (islas, en el código se llaman streams) de individuos evolucionan de manera independiente, intercambiando periódicamente individuos entre islas para mantener la diversidad y promover la exploración.
- **Mutación**: El proceso de introducir cambios aleatorios en el material genético (pesos, sesgos o estructura de la red) de los individuos en la población. La mutación ayuda a introducir nuevas variaciones y explorar el espacio de soluciones.

# Descripción General
### Configuración
- Cada serpiente tiene una red neuronal que actúa como su cerebro.
- La serpiente puede ver en 4 direcciones. Puede detectar comida, paredes y a sí misma en estas 4 direcciones. Número total de entradas = `4 * 3 = 12`.
- Estos 12 valores se alimentan como entrada a la red neuronal. La red neuronal luego genera 4 valores que indican el umbral para las acciones: izquierda, derecha, abajo y arriba.
- Cada generación tiene 5 streams (islas) de 1000 serpientes cada uno. Las serpientes en cada stream evolucionan independientemente de las serpientes de otros streams.
- Ocasionalmente, las serpientes con mejor rendimiento de un stream se inyectan en otro. Esta técnica se llama "Rejuvenecimiento de Islas".

### Algoritmo
- La simulación comienza en la `Generación 0` con 5 streams de juegos, los individuos en cada uno de estos streams tienen redes neuronales generadas aleatoriamente.
- En cada paso, actualizamos cada juego, es decir, pasamos las entradas de visión a la red neuronal y esta decide la acción a tomar.
- Cuando la serpiente colisiona con las paredes o consigo misma, el juego se marca como completado.
- Además, los juegos se marcan como completos cuando la serpiente no puede comer comida durante un cierto número de pasos. Esto es para evitar que las serpientes realicen acciones en bucle indefinidamente.
- Actualizamos cada juego en una generación hasta que todos los juegos estén completos.
- Al final de cada generación, cada serpiente en un stream se clasifica según su desempeño.
- Basado en esta clasificación, se eligen los padres para el próximo grupo de serpientes. La serpiente en el rango 1 tiene más probabilidades de ser un padre en comparación con la serpiente en el rango 10.
- Aquí hay un ejemplo de cómo se distribuye la población a lo largo de cada generación:
    1. El 1% superior de las serpientes se retiene para la próxima generación.
    2. El 50% de la población se genera usando 2 serpientes de la generación anterior como sus padres.
    3. El 20% de las serpientes se generan de nuevo sin conexión con las generaciones pasadas.
    4. El 29% restante de la población son todas mutaciones de las serpientes con mejor rendimiento actual.
- Una vez que tenemos una nueva población, comenzamos una nueva generación. Y los pasos anteriores se realizan hasta que la simulación se cierra manualmente.
- Los pasos anteriores resultan en que las serpientes afinan sus estrategias, lo que a su vez lleva a serpientes más largas.

# Uso
- Clonar el repositorio
```bash
git git@github.com:Antonio-Richaud/AI-snake.git
cd AI-snake
```
- Correr la simulación
```bash
cargo run --release
```

## Configurations
- El archivo de configuración del proyecto se encuentra en `src/configs.rs`
- Deshabilitar `VIZ_DARK_THEME` cambia el tema.
- La función de streams aún es experimental. Un solo stream con 1000 serpientes dará resultados rápidos.
