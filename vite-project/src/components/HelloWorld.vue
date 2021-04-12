<template>
  <div id="threejs">
    <p>three.js + Vue + vite + socket io tutorial.</p>
    <div>작성자의 이름을 입력하세요 :{{ name }}</div>
  </div>
</template>

<script lang="ts" type="modules">
// 모듈로 선언하면 관련 디펜던시 스크립트 다 가져온다.
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
    const app = window.document.querySelector("#threejs");
    const renderer = new THREE.WebGLRenderer();
    app?.appendChild(renderer.domElement);

    // 자바스크립트의 핵심 객체 : scene과 camera
    // 객체의 모임 : 3d모델을 구성하는 모든 객체의 부모
    const scene = new THREE.Scene();
    // 카메라 : 보이는 객체 따로 보이거나 같이 보이거나
    // 원근카메라 생성
    const camera = new THREE.PerspectiveCamera(
      75, // fov 필드오브뷰(시야각)
      innerWidth / innerHeight, // 가로 세로 비율
      0.1, // near 카메라가 찍는 공간의 범위 (가까운화면)
      1000 // far 카메라가 찍는 공간의 범위 (먼화면)
    );
    camera.position.z = 15;

    // 기하학 객체의 정점 데이터(재사용가능, 내장된 기본 데이터사용 가능)
    const radius = 1;
    const detail = 1;
    const geometry = new THREE.IcosahedronGeometry(radius, detail);
    // mesh의 도구(재사용가능, 표면의 속성 like texture- 랜더링된 이미지)
    const material = new THREE.MeshPhongMaterial({ color: 0xbfd9d7 });
    // 기하학 객체를 그리는 객체 (2번 그린다면 2개)
    // const mesh = new THREE.Mesh(geometry, material);
    // scene.add(mesh);

    // 광원추가
    const color = 0xfff2220;
    const intensity = 1;
    const light = new THREE.DirectionalLight(color, intensity);
    light.position.set(-1, 2, 4);
    scene.add(light);

    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(devicePixelRatio);
    renderer.render(scene, camera);

    function makeInstance(geometry: any, color: number, x: number) {
      const material = new THREE.MeshPhongMaterial({ color });
      
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);
      cube.position.x = x;
      return cube;
    }

    const cubes = [
      makeInstance(geometry, 0xbfd9d7, 0),
      makeInstance(geometry, 0xbfd9d7, -2),
      makeInstance(geometry, 0xbfd9d7, 2),
      makeInstance(geometry, 0xbfd9d7, 4),
      makeInstance(geometry, 0xbfd9d7, -4),
    ];

    animate();

    function resizeRendererToDisplaySize(
      renderer: THREE.WebGLRenderer
    ): boolean {
      const canvas = renderer.domElement;
      const pixelRatio = window.devicePixelRatio;
      const width = (canvas.clientWidth * pixelRatio) | 0;
      const height = (canvas.clientHeight * pixelRatio) | 0;
      const needResize = canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    }

    function animate(): void {
      requestAnimationFrame(animate);
      renderer.render(scene, camera);

      if (resizeRendererToDisplaySize(renderer)) {
        const canvas = renderer.domElement;
        camera.aspect = canvas.clientWidth / canvas.clientHeight; //비율 맞추고
        camera.updateProjectionMatrix(); // 창의 크기를 바꾸더라도 비율이 깨지지 않는다.
      }

      cubes.forEach((cube, ndx) => {
        cube.rotation.x += 0.021 + ndx * 0.0003;
        cube.rotation.y += 0.042 + ndx * 0.0021;
        cube.position.z += 0.0005;
      });
    }
  },
  methods: {},
});
</script>

<style scoped></style>
