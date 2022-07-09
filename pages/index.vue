<style scoped>
  * {
    margin: 0;
  }

  .index__page {
    width: 100%;
  }

  .index__ttl {
    text-align: center;
    padding: 30px 0;
  }

  .input__box {
    width: 100%;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .search__label {
    font-weight: bold;
    font-size: 20px;
    padding-right: 7px;
  }

  .search {
    width: 80%;
    height: 100%;
    font-size: 30px;
  }

  .kanji__list {
    width: 100%;
    list-style: none;
    margin-top: 30px;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }

  .kanji__list__item {
    width: 23%;
    height: 130px;
    line-height: 130px;
    text-align: center;
    font-size: 50px;
    margin-bottom: 10px;
    border: 1px solid #999999;
    border-radius: 5px;
    box-shadow: 3px 3px 3px 3px #999999;
  }

  .kanji__list__item:active {
    box-shadow: none;
    margin-left: 3px;
    margin-top: 3px;
  }


  @media screen and (max-width: 768px){
    .kanji__list__item {
      width: 46%;
      font-size: 30px;
    }
  }
</style>

<template>
  <div class="index__page">
    <h1 class="index__ttl">サクッと見つける君</h1>

    <div class="input__box">
      <label class="search__label" for="search">検索: </label>
      <input class="search" id="search" type="text" v-model="search" @change="getKanji">
    </div>

    <ul class="kanji__list">
      <li class="kanji__list__item" v-for="kanji in kanjiList">
        <p @click="openModal(kanji)">{{kanji}}</p>
      </li>
    </ul>

    <modal class="modal" :kanji="postItem" v-show="modalShow" @close="closeModal" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      kanjiList: [],
      search: "",
      modalShow: false,
      postItem: "",
    }
  },
  methods: {
    async getKanji() {
      const kanjiRegex = /([\u{3005}\u{3007}\u{303b}\u{3400}-\u{9FFF}\u{F900}-\u{FAFF}\u{20000}-\u{2FFFF}][\u{E0100}-\u{E01EF}\u{FE00}-\u{FE02}]?)/mu;

      const resKanjiData = await this.$axios.get(
        "http://www.google.com/transliterate?langpair=ja-Hira|ja&num=50&text=" + this.search
      );
      const kanjiArr = resKanjiData.data[0][1];

      const kanjiList = kanjiArr.filter(kanji => kanjiRegex.test(kanji));

      this.kanjiList = kanjiList;
    },
    openModal(item) {
      this.modalShow = true;
      this.postItem = item;
    },
    closeModal() {
      this.modalShow = false;
    }
  },
}
</script>