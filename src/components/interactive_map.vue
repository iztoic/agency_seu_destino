<template>
    <section>
      <div class="map-container" ref="mapContainer"></div>
    </section>
  </template>
  <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.49.0/mapbox-gl.js'></script>
  <script>
  import * as THREE from 'three';
  import { MapboxLayer } from 'mapbox-gl';
  import mapboxgl from 'mapbox-gl';
  
  export default {
    name: "InteractiveMap",
    data() {
      return {};
    },
    mounted() {
      mapboxgl.accessToken = 'pk.eyJ1IjoibnlrMTIzIiwiYSI6ImNrdmhmd25qdjJpa2sydXMxMHN3cTF6NTYifQ.S4nAVug9JCv9pCj8oww6wA';
      
      const mapContainer = this.$refs.mapContainer;
  
      // Configuração do mapa estilo globo
      const map1 = new mapboxgl.Map({
        container: mapContainer,
        style: 'mapbox://styles/mapbox/satellite-streets-v12',
        center: [-51.9253, -14.2350], // Centered on Brazil
        zoom: 1.8, // Definindo o zoom para ver o globo inteiro
        pitch: 0, // Alterado para 0 para remover a inclinação
        bearing: 0, // Alterado para 0 para remover a orientação
        renderWorldCopies: false, // Impede a duplicação do globo
        antialias: true, // Ativa o antialiasing para melhorar a qualidade visual
        projection: 'globe'
      });
  
      // Adicionando camada Three.js para representar o mapa em uma esfera
      map1.on('style.load', function () {
        const layer = new MapboxLayer({
          id: 'custom_layer',
          type: 'custom',
          renderingMode: '3d',
          onAdd: function (map, mbxContext) {
            this.camera = new THREE.PerspectiveCamera(60, map.getCanvas().clientWidth / map.getCanvas().clientHeight, 1, 1000); // Alterado para a perspectiva correta
            this.scene = new THREE.Scene();
  
            const geometry = new THREE.SphereGeometry(100, 32, 32); // Raio da esfera
            const material = new THREE.MeshBasicMaterial({
              map: new THREE.CanvasTexture(mbxContext.canvas)
            });
  
            const mesh = new THREE.Mesh(geometry, material);
            mesh.scale.set(-1, 1, 1); // Invertendo o lado interno da esfera
            this.scene.add(mesh);
  
            const width = map.getCanvas().clientWidth;
            const height = map.getCanvas().clientHeight;
            this.renderer = new THREE.WebGLRenderer({
              canvas: map.getCanvas(),
              context: mbxContext.gl,
              antialias: true,
              alpha: true
            });
            this.renderer.setSize(width, height);
          },
          render: function (gl, matrix) {
            const rotationX = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(1, 0, 0), matrix[0]);
            const rotationY = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(0, 1, 0), matrix[4]);
            const rotationZ = new THREE.Matrix4().makeRotationAxis(new THREE.Vector3(0, 0, 1), matrix[8]);
            const m = new THREE.Matrix4().fromArray(matrix);
            const l = new THREE.Matrix4().makeTranslation(0, 0, 5).multiply(rotationX).multiply(rotationY).multiply(rotationZ);
            this.camera.projectionMatrix = m.multiply(l);
            this.renderer.state.reset();
            this.renderer.render(this.scene, this.camera);
            map.triggerRepaint();
          }
        });
  
        map1.addLayer(layer);
      });
    }
  };
  </script>
  
  <style scoped>
  .map-container { width: 100%; height: 100vh; }
  </style>
  