<script>
import { beforeMain } from "@popperjs/core";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";


export default {
    data() {
        return {
            people: 500,
            people1: 300,
            people2: 99,
        };
    },
    mounted() {
        this.initScence()
    },
    methods: {
        async initScence() {
            // 場景
            const scene = new THREE.Scene();

            // 相機
            const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.x = 0;
            camera.position.y = 0;
            camera.position.z = 5;

            // 炫染畫面
            const renderer = new THREE.WebGLRenderer();
            renderer.setSize(window.innerWidth, window.innerHeight);
            this.$refs.scene.appendChild(renderer.domElement);

            // 軌道控制
            // 要Import
            new OrbitControls(camera, renderer.domElement);

            // 使用TextureLoader載入PNG圖片
            const textureLoader = new THREE.TextureLoader();
            const texture = textureLoader.load('/可愛地圖.png');

            // 可以讓圖片黑的地方變立體
            const displacementMap = textureLoader.load('/456.png');

            // 模型
            // 添加網格數，32和32是示例值
            const geometry = new THREE.PlaneGeometry(5, 5, 32, 32);
            const material = new THREE.MeshPhongMaterial({
                map: texture,
                displacementMap: displacementMap,
                displacementScale: -0.3,
            });
            const plane = new THREE.Mesh(geometry, material);
            scene.add(plane);

            // 人數分布
            // 雲霄飛車

            const originGeometry = new THREE.SphereGeometry(0.1);
            let originMaterial
            if (this.people >= 500) {
                originMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 })
            } else if (this.people >= 300) {
                originMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 })
            } else {
                originMaterial = new THREE.MeshBasicMaterial({ color: 0x3cb371 })
            }
            const originPoint = new THREE.Mesh(originGeometry, originMaterial)
            originPoint.position.set(-1.7, 0.2, 0)

            // 摩天輪
            const originGeometry1 = new THREE.SphereGeometry(0.1);
            let originMaterial1
            if (this.people1 >= 500) {
                originMaterial1 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
            } else if (this.people1 >= 300) {
                originMaterial1 = new THREE.MeshBasicMaterial({ color: 0xffff00 })
            } else {
                originMaterial1 = new THREE.MeshBasicMaterial({ color: 0x3cb371 })
            }
            const originPoint1 = new THREE.Mesh(originGeometry1, originMaterial1)
            originPoint1.position.set(1, -0.1, 0)

            // 小馬
            const originGeometry2 = new THREE.SphereGeometry(0.1);
            let originMaterial2
            if (this.people1 >= 500) {
                originMaterial2 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
            } else if (this.people1 >= 300) {
                originMaterial2 = new THREE.MeshBasicMaterial({ color: 0xffff00 })
            } else {
                originMaterial2 = new THREE.MeshBasicMaterial({ color: 0x3cb371 })
            } const originPoint2 = new THREE.Mesh(originGeometry2, originMaterial2)
            originPoint2.position.set(0, 0.3, 0)

            // 火山歷險
            const originGeometry3 = new THREE.SphereGeometry(0.1);
            let originMaterial3
            if (this.people2 >= 500) {
                originMaterial3 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
            } else if (this.people2 >= 300) {
                originMaterial3 = new THREE.MeshBasicMaterial({ color: 0xffff00 })
            } else {
                originMaterial3 = new THREE.MeshBasicMaterial({ color: 0x3cb371 })
            } const originPoint3 = new THREE.Mesh(originGeometry3, originMaterial3)
            originPoint3.position.set(1.1, -1.2, 0)

            // 冰雪奇緣
            const originGeometry4 = new THREE.SphereGeometry(0.1);
            let originMaterial4
            if (this.people1 >= 500) {
                originMaterial4 = new THREE.MeshBasicMaterial({ color: 0xff0000 })
            } else if (this.people1 >= 300) {
                originMaterial4 = new THREE.MeshBasicMaterial({ color: 0xffff00 })
            } else {
                originMaterial4 = new THREE.MeshBasicMaterial({ color: 0x3cb371 })
            } const originPoint4 = new THREE.Mesh(originGeometry4, originMaterial4)
            originPoint4.position.set(0.4, -1, 0)

            scene.add(originPoint, originPoint1, originPoint2, originPoint3, originPoint4)

            //閃爍 秒數
            const blinkInterval = 800;

            // 設定一個 flag 來跟蹤 originPoint4 的狀態
            let isHidden = false;

            // 使用 setInterval 創建閃爍效果
            setInterval(() => {
                if (isHidden) {
                    originPoint.visible = true;
                    originPoint1.visible = true;
                    originPoint2.visible = true;
                    originPoint3.visible = true;
                    originPoint4.visible = true;
                } else {
                    originPoint.visible = false;
                    originPoint1.visible = false;
                    originPoint2.visible = false;
                    originPoint3.visible = false;
                    originPoint4.visible = false;
                }

                // 切換 flag 狀態
                isHidden = !isHidden;
            }, blinkInterval);

            // 座標
            // const axis = new THREE.AxesHelper();
            // scene.add(axis)

            // 環境光
            const ambientLight = new THREE.AmbientLight(0xffffff, 3);
            scene.add(ambientLight);

            // 光 16進制的色號
            const light = new THREE.PointLight(0xb22222);
            light.position.set(10, 10, 10);
            scene.add(light);

            // 循環
            const animate = (callback) => {
                // requestAnimationFrame(animate);

                // 讓模型旋轉
                if (callback && typeof callback === "function") callback();
                requestAnimationFrame(() => {
                    animate(callback);
                });
                // callback.rotation.x += 0;
                // callback.rotation.y += 0.01;

                // 渲染場景
                renderer.render(scene, camera);
            };

            animate();

            // 畫面大小
            window.addEventListener('resize', () => {
                const newWidth = window.innerWidth;
                const newHeight = window.innerHeight;

                camera.aspect = newWidth / newHeight;
                camera.updateProjectionMatrix();

                renderer.setSize(newWidth, newHeight);
            });
        },
    },
};

</script>

<template>
    <div ref="scene" class="top"></div>
</template>

<style scoped>

</style> 