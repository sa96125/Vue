<template>
  <div id="threejs">
    <p>three.js + Vue + vite + socket io tutorial.</p>
    <div>작성자의 이름을 입력하세요 :{{ name }}</div>
  </div>
</template>

<script lang="ts">
import { defineComponent, toRef, ref, reactive, render } from "vue";
import * as THREE from "three";

// 새로운 타입 정의 : type과 interface의 활용도에 대해서 알고 있자.
// 선언적으로 차이가 있지만 타입스크립트에서 사용성은 동일하다고 봐도 무방하다.
// 단지 interface의 경우에는 emerge(합친다)가 가능하다.
type Quux = {
  quuz: string;
  corge: number;
};
interface Quux2 {
  quuz: string;
}
interface Quux2 {
  corge: number;
}

export default defineComponent({
  name: "HelloWorld",
  props: {
    title: String,
  },
  // 셋업함수는 data 함수 외의 연결을 셋팅하는 작업을 하는 공간이다.
  // setup(props, { attrs, slots, emit }) {
  //   const title = toRef(props, "title");
  //   const readersNumber = ref(0);
  //   const book = reactive({ title: "Vue 3 Guide" });

  //   return {
  //     readersNumber,
  //     book,
  //   };
  // },
  data(): object {
    return {
      name: "박종승",
    };
  },
  // 렌더링 이후의 사이클
  mounted(): void {
    // 객체의 모임 : 3d모델을 구성하는 모든 객체의 부모
    const scene = new THREE.Scene();
    // 카메라 : 보이는 객체 따로 보이거나 같이 보이거나
    const camera = new THREE.PerspectiveCamera(
      75,
      innerWidth / innerHeight,
      0.1,
      1000
    );
    // 자바스크립트의 핵심 객체 : scene과 camera
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(devicePixelRatio);

    const app = window.document.querySelector("#threejs");
    app?.appendChild(renderer.domElement);

    const boxGeometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    const mesh = new THREE.Mesh(boxGeometry, material);
    scene.add(mesh);

    camera.position.z = 5;
    renderer.render(scene, camera);

    function animate(): void {
      requestAnimationFrame(animate);
      renderer.render(scene, camera);
      mesh.rotation.x += 0.01;
      mesh.rotation.y += 0.01;
      mesh.rotation.z += 0.01;
    }
    animate();
  },
  methods: {},
});
</script>

<style scoped></style>
