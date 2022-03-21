<template>
  <div class="header">
    <ul class="header-button-left">
      <li @click="tab_number=0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="tab_number==0 || tab_number==1" @click="tab_number++">Next</li>
      <li v-if="tab_number==2" @click="publish">submit</li>
    </ul>
    <img src="./assets/logo.png" class="logo"/>
  </div>

  <div class="menu" style="text-align: center">
    <button @click="tab_number=0">게시글</button>
    <button @click="tab_number=1">필터선택</button>
    <button @click="tab_number=2">글작성</button>
  </div>
  <Container @writer="name=$event" @board_text="boardText=$event" :url="url" :tab_number="tab_number" @input_result_data="data.push($event)" :data="data"/>

  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" multiple type="file" accept="image/*" id="file" class="inputfile"/>
       <!-- * 참고. 업로드 기능 구현할 input 타입에 multiple 코드를 추가하면 여러개의 파일을 선택할 수 있음 -->
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>
</template>

<script>
import Data from "@/assets/postData";
import Container from "@/components/Container";

export default {
  name: 'App',
  data() {
    return {
      data: [...Data],
      tab_number: 0,
      url: '',
      name: '',
      boardText: '',
    }
  },
  components: {
    Container
  },
  methods: {
    upload(e) { //e는 event와 관련된 여러가 함수가 담겨 있는 변수(자료 변수)
      let file = e.target.files; //내가 업로드한 파일 다 담겨 있다(e.target.files)
      for(let i=0; i<file.length; i++) {
        console.log(file[i].type); //array 내 i번째 요소의 type 알아보기
        let image_url = URL.createObjectURL(file[i]); //업로드한 이미지의 url 생성을 위한 코드
        console.log(this.url);
        this.url = image_url;
      }
      //* console 창 내 위 url 중 blob 이라는 내용에 대해서 알아보기 (blob은 데이터를 다룰 때 blob이라는 object에 대해서 다룸)
      this.tab_number++; //업로드 하는 순간 다음 tab으로 넘어가기 위한 코드
    },
    publish() {
      let today = new Date();
      let year = today.getFullYear();
      let month = today.getMonth()+1;
      let day = today.getDate();

      let my_post = {
        name: this.name,
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.url,
        likes: 36,
        date: year + '-' + month + '-' + day ,
        liked: false,
        content: this.boardText,
        filter: "perpetua"
      };
      this.data.unshift(my_post); //unshift 함수는 array 데이터형의 가장 왼쪽 index에 데이터를 넣는 코드
      this.tab_number = 0; //게시물을 발행한 후에 post 게시물을 보여주는 화면으로 이동
    }
  }
}
</script>

<style>
body {
  margin: 0;
}

ul {
  padding: 5px;
  list-style-type: none;
}

.logo {
  width: 30px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}

.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}

.header-button-left {
  color: black;
  font-weight: bold;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}

.header-button-right {
  color: black;
  font-weight: bold;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}

.menu button {
  margin-right: 10px;
  border-width: 2px;
}

.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}

.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}

.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}

.inputfile {
  display: none;
}

.input-plus {
  cursor: pointer;
}

#app {
  box-sizing: border-box;
  font-family: "consolas";
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-style: dotted;
  border-width: 1px;
}
</style>
