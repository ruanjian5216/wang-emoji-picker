<template>
  <div class="wang-emoji-picker">
    <!-- title区域 -->
    <div class="title_box">Smileys & {{ activeMenuItem.name }}</div>

    <!-- 表情选择区 -->
    <div class="emoji_list_box">
      <div
        v-for="(item, index) in emojiTmpArr"
        :key="index"
        class="emoji_item"
        v-html="item.source"
        @click="emojiClickItem(item)"
      ></div>
    </div>

    <!-- 菜单 -->
    <div class="menu_box">
      <div
        v-for="item in menuArr"
        :key="item.icon.unicode"
        class="emoji_menu_item"
        @click="slectMenuHandle(item)"
      >
        {{ item.icon.char }}
      </div>
    </div>
  </div>
</template>

<script>
import emojis from "./emojis.json";
export default {
  data() {
    return {
      emoji: emojis,
      menuArr: [],
      emojiTmpArr: [],
      activeMenuItem: {}, //默认选中的表情对象
    };
  },
  methods: {
    // 解析 emoji
    parseEmoji(data) {
      return twemoji.parse(data);
    },
    //   菜单点击
    slectMenuHandle(item) {
      let emojisArr = this.emoji.find(
        (itemx) => itemx.name === item.name
      ).emojis;

      this.activeMenuItem = item;
      this.emojiTmpArr = this.formatData(emojisArr);
    },
    // 表情点击
    emojiClickItem(item) {
      // 提取图片源地址
      var reg = /<[\s\S]*?src="([^\s]+)".*/;
      var result = item.source.match(reg);
      let imgUrl = result && result[1];
      this.$emit("emojiClickItem", { ...item, imgUrl });
    },
    // 格式数据
    formatData(emojisArr) {
      return emojisArr.map((itemx) => {
        return {
          source: this.parseEmoji(itemx.char),
          ...itemx,
        };
      });
    },
  },
  mounted() {
    let emoji = this.emoji;

    // 菜单数组
    this.menuArr = emoji.map((item) => {
      return {
        name: item.name,
        icon: item.icon,
      };
    });

    // 首次
    this.emojiTmpArr = this.formatData(emoji[0].emojis);
    this.activeMenuItem = emoji[0];

    setTimeout(() => {
      twemoji.parse(document.body);
    });
  },
};
</script>

<style lang="less" scoped>
.wang-emoji-picker {
  width: 254px;
  height: 300px;
  background-color: #ccc;
  overflow: hidden;
  user-select: none;
  ::v-deep img {
    height: 18px;
    width: 18px;
    margin: 4px;
  }
  /* title区域 */
  .title_box {
    height: 40px;
    display: flex;
    align-items: center;
    background-color: #f3f6ff;
    padding: 0 12px;
  }
  /* 菜单区域 */
  .menu_box {
    display: flex;
    align-items: center;
    height: 52px;
    padding: 0 8px;
    background-color: #f3f6ff;
    box-sizing: border-box;
    .emoji_menu_item {
      cursor: pointer;
      flex: 1;
    }
  }
}

// 表情列表
.emoji_list_box {
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  justify-content: flex-start;
  height: calc(100% - 40px - 50px);
  overflow: auto;
  padding: 5px 8px;
  box-sizing: border-box;
  background-color: #fff;
  .emoji_item {
    display: flex;
    justify-content: center;
    cursor: pointer;
  }
  &::-webkit-scrollbar {
    width: 4px; /*高宽分别对应横竖滚动条的尺寸*/
    height: 0px;
  }
  &::-webkit-scrollbar-thumb {
    border-radius: 10px;
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    background: rgba(0, 0, 0, 0.2);
    -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    background: rgba(0, 0, 0, 0.2);
  }
  &::-webkit-scrollbar-track {
    box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    border-radius: 0;
    background: rgba(0, 0, 0, 0.1);
    -webkit-box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
    border-radius: 0;
    background: rgba(0, 0, 0, 0.1);
  }
}
</style>