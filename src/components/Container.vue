<template>
  <div>
    <!-- 게시글 페이지 -->
    <div class="post-page" v-if="tab_number==0">
      <Post @input_result_data="input_data=$event" :click_count="click_count" :data="one_data"
            v-for="(one_data,count) in data" :key="count"/>
      <button @click="more(click_count)">더보기</button>
    </div>

    <!--필터 선택 페이지-->
    <div class="filter-select-page" style="margin-top: 10px" v-if="tab_number==1">
      <div class="upload-image" :style="{'background-image': 'url(' + url + ')'}"></div>
      <!-- style을 v-bind(:) 하여 background-image 내에 이미지 url 넣어서 업로드 된 이미지 출력 -->
      <div class="filters">
        <div class="filter-1"></div>
        <div class="filter-1"></div>
        <div class="filter-1"></div>
        <div class="filter-1"></div>
        <div class="filter-1"></div>
      </div>
    </div>

    <!-- 글작성페이지 -->
    <div class="write-page" style="margin: 10px" v-if="tab_number==2">
      <div class="upload-image" :style="{'background-image': 'url(' + url + ')'}"></div>
      <div class="write">
        작성자: <input :value="writer" @input="$emit('writer',writer=$event.target.value)"/>
        <textarea class="write-box" :value="board_text" @input="$emit('board_text',board_text=$event.target.value)">글을 작성해주세요.</textarea>

      </div>
    </div>
  </div>
</template>

<script>
import Post from "@/components/Post";
import axios from "axios";

export default {
  name: 'Container',
  props: {
    data: Array,
    tab_number: Number,
    url: String,
  },
  data() {
    return {
      input_data: {},
      result_data: Object,
      click_count: 0,
      writer: '',
      board_text: '',
    }
  },
  components: {
    Post,
  },
  methods: {
    more(count) {
      axios.get('https://codingapple1.github.io/vue/more'+count+'.json')
          .then( result => {
                this.result_data = result.data;
                console.log(this.result_data.name);
                this.$emit('input_result_data', this.result_data);
                this.click_count++;
                console.log(this.click_count);
              }

          ).catch(function () {
              alert('데이터가 존재하지 않습니다.');
              return;
        }
      );
    }
  },
  beforeUpdate() {
    console.log(this.url);
  },
}
</script>

<style>

.write {
  margin-top: 10px;
}
.write textarea {
  border-style: solid;
  border-width: 1px;
  margin-top: 10px;

}
.upload-image {
  width: 100%;
  height: 450px;
  background: cornflowerblue;
  background-size: cover;
}

.filters {
  overflow-x: scroll;
  white-space: nowrap;
}

.filter-1 {
  width: 100px;
  height: 100px;
  background-color: cornflowerblue;
  margin: 10px 10px 10px auto;
  padding: 8px;
  display: inline-block;
  color: white;
  background-size: cover;
}

.filters::-webkit-scrollbar {
  height: 5px;
}

.filters::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.filters::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 5px;
}

.filters::-webkit-scrollbar-thumb:hover {
  background: #555;
}

.write-box {
  border: none;
  width: 90%;
  height: 100px;
  padding: 15px;
  margin: auto;
  display: block;
  outline: none;
}
</style>