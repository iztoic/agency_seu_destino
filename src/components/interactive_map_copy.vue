<template>
  <div>
    <div id="map"></div>
    <div id="controls">
      <div class="prev cta" @click="moveToPrevious">&#9664;</div>
      <div class="next cta" @click="moveToNext">&#9654;</div>
    </div>
    <div id="timeline">
      <div class="helper">
        <span
          v-for="(datum, index) in geoJSONData.features"
          :key="index"
          class="date"
          @click="moveToIndex(index)"
        >
          <span class="label">{{ datum.properties.date }}</span>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from 'mapbox-gl';
import gsap from 'gsap';
import turf from '@turf/turf';

export default {
  // Definição do nome do componente
  name: "InteractiveMap",
  // Declaração dos dados do componente
  data() {
    return {
      geoJSONData: {
        // Dados geoJSON para os pontos do mapa
        type: 'FeatureCollection',
        features: [
          // Lista de pontos com suas propriedades
          {
            type: 'Feature',
            properties: { name: 'Joinville', date: '20/09/2024' },
            geometry: { type: 'Point', coordinates: [-48.8489, -26.3045] }
          },
          {
            type: 'Feature',
            properties: { name: 'Curitiba', date: '20/09/2024' },
            geometry: { type: 'Point', coordinates: [-49.2733, -25.4284] }
          },
          {
            type: 'Feature',
            properties: { name: 'São Paulo', date: '20/09/2024', type: 'stopover' },
            geometry: { type: 'Point', coordinates: [-46.6333, -23.5505] }
          },
          {
            type: 'Feature',
            properties: { name: 'Madri', date: '21/09/2024' },
            geometry: { type: 'Point', coordinates: [-3.7038, 40.4168] }
          },
          {
            type: 'Feature',
            properties: { name: 'Abu Dhabi', date: '22/09/2024' },
            geometry: { type: 'Point', coordinates: [54.3773, 24.4539] }
          },
          {
            type: 'Feature',
            properties: { name: 'Sidney', date: '23/09/2024' },
            geometry: { type: 'Point', coordinates: [151.2093, -33.8688] }
          }
        ]
      },
      // Índice do ponto atual
      idx: 0,
      // Objeto do mapa
      map: null,
      // Marcador no mapa
      marker: null,
      // Array de posições para animação da linha do tempo
      snaps: [],
      // Timeline animada
      tl: null,
      // Elementos DOM relevantes
      $timelineHelper: null,
      $prev: null,
      $next: null,
      $title: null
    };
  },
  // Método executado após a montagem do componente
  mounted() {
    // Inicialização dos elementos DOM
    this.$prev = document.querySelector('.prev');
    this.$next = document.querySelector('.next');
    this.$title = document.querySelector('#title');
    this.$timelineHelper = document.querySelector('#timeline .helper');

    // Configuração inicial do título
    this.setTitle(this.geoJSONData.features[this.idx].properties.name);

    // Adiciona listeners para os botões de navegação
    this.$next.addEventListener('click', this.moveToNext);
    this.$prev.addEventListener('click', this.moveToPrevious);

    this.snaps = gsap.utils.toArray('.date').map(date => window.innerWidth / 2 - date.offsetLeft);
    // Inicialização do mapa
    mapboxgl.accessToken = 'pk.eyJ1IjoibnlrMTIzIiwiYSI6ImNrdmhmd25qdjJpa2sydXMxMHN3cTF6NTYifQ.S4nAVug9JCv9pCj8oww6wA';
    this.map = new mapboxgl.Map({
      // Configuração do mapa
      container: 'map',
      style: 'mapbox://styles/mapbox/satellite-streets-v12',
      center: this.geoJSONData.features[this.idx].geometry.coordinates,
      zoom: 2,
      antialias: true
    });

    // Evento de carregamento do estilo do mapa
    this.map.on('style.load', () => {
      // Lógica para adicionar camadas e fontes ao mapa
      const layers = this.map.getStyle().layers;
      let firstSymbolId;
      for (const layer of layers) {
        if (layer.type === 'symbol') {
          firstSymbolId = layer.id;
          break;
        }
      }

      this.map.addSource('sites', { type: 'geojson', data: this.geoJSONData });

      this.map.addSource('route', {
        type: 'geojson',
        data: {
          type: 'Feature',
          properties: {},
          geometry: {
            type: 'LineString',
            coordinates: this.geoJSONData.features.map((_, index) => {
              const { coordinates } = _.geometry;
              const [x, y] = coordinates;
              return index < 10 ? [x, y] : [x - 360, y];
            })
          }
        }
      });

      this.map.addLayer({
        id: 'route',
        type: 'line',
        source: 'route',
        layout: { 'line-join': 'round', 'line-cap': 'round' },
        paint: { 'line-color': '#888', 'line-width': 2 }
      }, firstSymbolId);

      this.map.addLayer({
        id: 'sites',
        type: 'circle',
        source: 'sites',
        paint: { 'circle-radius': 6, 'circle-color': '#ff5b00', 'circle-opacity': 1, 'circle-stroke-color': '#7c7d7e', 'circle-stroke-width': 2 }
      }, firstSymbolId);

      this.map.addLayer({
        id: 'site-labels',
        type: 'symbol',
        source: 'sites',
        layout: { 'text-field': ['get', 'name'], 'text-variable-anchor': ['top', 'bottom', 'left', 'right'], 'text-radial-offset': 1, 'text-justify': 'auto' },
        paint: { 'text-color': '#202', 'text-halo-color': '#fff', 'text-halo-width': 1.5 }
      });
    });

    this.$timelineHelper.style.width = this.snaps[this.idx] + 'px';

    const draggable = Draggable.create(this.$timelineHelper, {
      type: 'x',
      trigger: this.$timeline,
      inertia: true,
      snap: { x: this.snaps },
      onDragEnd: () => {
        const newIdx = this.snaps.indexOf(draggable[0].endX);
        this.moveToIndex(newIdx, true);
      }
    });

    window.addEventListener('resize', () => {
      this.snaps = gsap.utils.toArray('.date').map(date => window.innerWidth / 2 - date.offsetLeft);
      draggable[0].vars.snap.x = this.snaps;
      this.$timelineHelper.style.width = this.snaps[this.idx] + 'px';
    });

    const el = document.createElement('div');
    el.className = 'marker';
    const img = document.createElement('img');
    img.src = 'airplane.png';
    img.style.width = '40px';
    img.style.height = '40px';
    el.appendChild(img);
    el.style.width = '30px';
    el.style.height = '30px';
    el.style.textAlign = 'center';
    this.marker = new mapboxgl.Marker(el).setLngLat(this.geoJSONData.features[this.idx].geometry.coordinates).addTo(this.map);

    const lineString = this.geoJSONData.features.map((_, index) => {
      const { coordinates } = _.geometry;
      const [x, y] = coordinates;
      return index < 10 ? [x, y] : [x - 360, y];
    });

    this.tl = new gsap.timeline({ paused: true });
    lineString.forEach((coords, index) => {
      if (index < this.geoJSONData.features.length - 1) {
        const nextCoords = lineString[index + 1];
        var projectedCoords = turf.toWgs84(turf.point(coords));
        var projectedNextCoords = turf.toWgs84(turf.point(nextCoords));
        var line = turf.lineString([projectedCoords.geometry.coordinates, projectedNextCoords.geometry.coordinates]);
        var length = turf.length(line);
        const attrs = { delta: 0 };
        this.tl.to(attrs, {
          delta: length,
          onUpdate: () => {
            var along = turf.toMercator(turf.along(line, attrs.delta));
            this.marker.setLngLat(along.geometry.coordinates);
          }
        }, `step-${index}`);
      }
    });
  },
  methods: {
    // Método para mover para um índice específico
    moveToIndex(newIdx, ignoreDrag = false) {
      // Lógica para movimento entre pontos no mapa
  // Verifica se newIdx está dentro dos limites do array de características
  if (newIdx >= 0 && newIdx < this.geoJSONData.features.length) {
    const { coordinates } = this.geoJSONData.features[newIdx].geometry;
    const [x, y] = coordinates;
    const center = [x, y];

    // Ajusta a lógica para lidar com índices especiais
    if (newIdx === 10 && this.idx === 9) center[0] -= 360;
    if (newIdx === 9 && this.idx === 10) center[0] += 360;

    this.map.flyTo({ duration: 1000, essential: true, center });
    this.setTitle(this.geoJSONData.features[newIdx].properties.name);
    if (!ignoreDrag) gsap.to(this.$timelineHelper, { x: this.snaps[newIdx] });
    gsap.utils.toArray('.date').forEach(_ => _.classList.remove('active'));
    if (this.tl) {
      if (newIdx === 0 && this.idx === this.geoJSONData.features.length - 1) {
        gsap.killTweensOf(this.tl);
        this.tl.seek(0, false);
      } else if (newIdx === this.geoJSONData.features.length - 1 && this.idx === 0) {
        gsap.killTweensOf(this.tl);
        this.tl.seek(this.tl.duration(), false);
      } else {
        this.tl.tweenTo(`step-${newIdx}`, { duration: 1, ease: 'sine.inOut' });
      }
    }
    gsap.utils.toArray('.date')[newIdx].classList.add('active');

    // Atualiza this.idx para newIdx após as operações
    this.idx = newIdx;
  }
},
    // Método para definir o título do ponto atual
    setTitle(text) {
      // Lógica para animação do título
  if (this.$title) {
    const tl = gsap.timeline();
    tl.to(this.$title, {
      alpha: 0,
      duration: 0.3,
      onComplete: () => {
        this.$title.innerHTML = text;
        const splits = new SplitText(this.$title, { type: 'words,chars' });
        gsap.set(this.$title, { alpha: 1 });
        gsap.set(splits.chars, { alpha: 0 });
        tl.to(splits.chars, { alpha: 0.314, stagger: 0.05 });
      }
    }, 'a');
  }
    },


    // Método para mover para o próximo ponto
    moveToNext() {
      // Lógica para mover para o próximo ponto na linha do tempo
      const newIdx = (this.idx + 1) % this.geoJSONData.features.length;
      this.moveToIndex(newIdx);
    },

    // Método para mover para o ponto anterior
    moveToPrevious() {
      // Lógica para mover para o ponto anterior na linha do tempo
      const newIdx = (this.idx === 0 ? this.geoJSONData.features.length : this.idx) - 1;
      this.moveToIndex(newIdx);
    }
  }
};
</script>

<style scoped>
#map { position: absolute; inset: 0; }
#controls {
  position: fixed;
  top: 50%;
  height: 140px;
  left: 0;
  right: 0;
  margin-top: -70px;
  pointer-events: none;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 2vw;
  font-size: 64px;
}
#controls .cta {
  pointer-events: all;
  cursor: pointer;
  opacity: 0.614;
  user-select: none;
}
#controls .cta:hover {
  opacity: 1;
}
#timeline {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 50px;
}
#timeline:hover {
  opacity: 1;
}
#timeline:before {
  content: "";
  opacity: 0.314;
  display: block;
  position: relative;
  top: 25px;
}
#timeline .helper {
  display: flex;
}
#timeline .date {
  width: 20px;
  display: block;
  transform: rotate(-70deg);
  transform-origin: 0 50%;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.1em;
  font-family: sans-serif;
}
#timeline .date .label {
  opacity: 0.314;
  transition: opacity 0.3s linear, transform 0.3s ease;
  text-shadow: 0 0 4px white,  0 0 4px white;
  display: block;
}
#timeline .date:before {
  content: "";
  display: block;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  position: absolute;
  right: 100%;
  top: 4px;
  margin-right: 14px;
  background-color: #afafaf;
}
#timeline .date.active .label {
  opacity: 0.75;
  transform: translateX(5px);
}
#timeline .date.active:before {
  background-color: #000;
}
#timeline .date:hover .label {
  opacity: 1;
}
</style>
