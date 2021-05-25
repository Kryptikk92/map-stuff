<template>
    <div ref="map-root" style="width: 100%; height: 100%"></div>
</template>

<script>
import View from 'ol/View'
import Map from 'ol/Map'
import TileLayer from 'ol/layer/Tile'
import OSM from 'ol/source/OSM'
import VectorLayer from 'ol/layer/Vector'
import VectorSource from 'ol/source/Vector'
import GeoJSON from 'ol/format/GeoJSON'

import 'ol/ol.css'

export default {
    name: 'MapContainer',
    props: {
        geoJson: Object
    },
    data: () => ({
        olMap: null,
        vectorLayer: null
    }),
    mounted() {
        this.vectorLayer = new VectorLayer({
            source: new VectorSource({
                features: []
            })
        });
        this.olMap = new Map({
            target: this.$refs['map-root'],
            layers: [
                new TileLayer({
                    source: new OSM()
                }),
                this.vectorLayer
            ],
            view: new View({
                zoom: 0,
                center: [0,0],
                constraintResolution: true
            })
        });
        this.olMap.on('pointermove', (event) => {
            const hovered = this.olMap.forEachFeatureAtPixel(
                event.pixel,
                (feature) => feature
            )
            this.$emit('select', hovered)
        })
        this.updateSource(this.geoJson)
    },
    watch: {
        geoJson(value) {
            this.updateSource(value)
        }
    },
    methods: {
        updateSource(geoJson) {
            const view = this.olMap.getView()
            const source = this.vectorLayer.getSource()

            const features = new GeoJSON({
                featureProjection: 'EPSG:3857'
            }).readFeatures(geoJson)
            source.clear();
            source.addFeatures(features);
            view.fit(source.getExtent())
        }
    }
}
</script>