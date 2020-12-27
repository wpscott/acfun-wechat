<template>
  <div id="app">
    <header>{{ title }}({{ audience }})</header>
    <div id="wrapper">
      <template v-for="item of list">
        <div
          :key="`${item.type}-${item.timestamp}-${item.user.id}`"
          v-if="item.type === 'comment'"
          class="row comment"
          :class="{ self: item.isSelf }"
        >
          <div class="avatar">
            <img :src="item.user.avatar" />
          </div>
          <div class="content">
            <p class="nickname">{{ item.user.nickname }}</p>
            <div class="comment">
              {{ item.content }}
            </div>
          </div>
        </div>
        <div
          :key="`${item.type}-${item.timestamp}-${item.user.id}`"
          v-else-if="item.type === 'enter'"
          class="row enter"
        >
          "<span class="nickname">{{ item.user.nickname }}</span
          >"加入了群聊
        </div>
        <div
          :key="`${item.type}-${item.timestamp}-${item.user.id}`"
          v-else-if="item.type === 'like'"
          class="row like"
        >
          "<span class="nickname">{{ item.user.nickname }}</span
          >"点赞了❤️
        </div>
        <div
          :key="`${item.type}-${item.timestamp}-${item.user.id}`"
          v-else-if="item.type === 'follow'"
          class="row follow"
        >
          "<span class="nickname">{{ item.user.nickname }}</span
          >"关注了你💛
        </div>
        <div
          :key="`${item.type}-${item.timestamp}-${item.user.id}`"
          v-else-if="item.type === 'gift'"
          class="row gift"
        >
          <div class="avatar">
            <img :src="item.user.avatar" />
          </div>
          <div class="content">
            <p class="nickname">{{ item.user.nickname }}</p>
            <div class="gift">
              <div class="icon">
                <img :src="item.gift.detail.img" />
              </div>
              <div class="detail">
                {{ item.gift.detail.name }} × {{ item.gift.count }}
              </div>
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      dmk: null,
      title: "标题",
      audience: 1,
      list: [],
    };
  },
  mounted() {
    const id = setInterval(() => {
      const it = (Math.random() * 5) | 0;
      switch (it) {
        case 0:
          this.list.push({
            type: "comment",
            content: `test ${+new Date()} ${+new Date()} ${+new Date()} `,
            timestamp: new Date(),
            user: {
              id: (new Date() * Math.random()) | 0,
              nickname: "test",
              avatar:
                "https://tx-free-imgs.acfun.cn/FpUZL492VnnNC8T2XPAplXLT9eyc",
            },
            isSelf: Math.random() < 0.5,
          });
          break;
        case 1:
          this.list.push({
            type: "like",
            timestamp: new Date(),
            user: {
              id: (new Date() * Math.random()) | 0,
              nickname: "test",
              avatar:
                "https://tx-free-imgs.acfun.cn/FpUZL492VnnNC8T2XPAplXLT9eyc",
            },
          });
          break;
        case 2:
          this.list.push({
            type: "enter",
            timestamp: new Date(),
            user: {
              id: (new Date() * Math.random()) | 0,
              nickname: "test",
              avatar:
                "https://tx-free-imgs.acfun.cn/FpUZL492VnnNC8T2XPAplXLT9eyc",
            },
          });
          break;
        case 3:
          this.list.push({
            type: "follow",
            timestamp: new Date(),
            user: {
              id: (new Date() * Math.random()) | 0,
              nickname: "test",
              avatar:
                "https://tx-free-imgs.acfun.cn/FpUZL492VnnNC8T2XPAplXLT9eyc",
            },
          });
          break;
        case 4:
          this.list.push({
            type: "gift",
            timestamp: new Date(),
            user: {
              id: (new Date() * Math.random()) | 0,
              nickname: "test",
              avatar:
                "https://tx-free-imgs.acfun.cn/FpUZL492VnnNC8T2XPAplXLT9eyc",
            },
            gift: {
              detail: {
                id: 1,
                name: "香蕉",
                img:
                  "http://static.yximgs.com/bs2/giftCenter/giftCenter-20200316101519KncIIcdd.png",
              },
              value: 1,
              count: (Math.random() * 5) | (0 + 1),
              comboId: "",
              comboCount: 1,
            },
          });
          break;
      }
      if (this.list.length > 100) {
        clearInterval(id);
      }
      this.audience = (1000 * Math.random()) | 0;
    }, 500);
    this.dmk = document.getElementById("wrapper");
  },

  updated() {
    this.dmk.scrollIntoView(false);
  },
};
</script>

<style lang="scss">
$padding: 16px;
$borderRadius: 8px;

$fontSize: 1.4rem;
$backgroundColor: #eee;
$selfColor: #5d5;
$giftColor: #f80;
$nicknameColor: #08f;

* {
  scrollbar-width: none;
  -ms-overflow-style: none;
}
::-webkit-scrollbar {
  display: none;
  width: 0;
  height: 0;
}

body {
  margin: 0;
  background-color: $backgroundColor;
}
#app {
  width: 100vw;
  height: 100vh;

  header {
    z-index: 999;
    position: fixed;
    top: 0;
    left: 0;
    width: calc(100vw - #{$padding * 2});
    height: 48px;
    &::before {
      content: "<";
      float: left;
    }
    &::after {
      content: "\2026";
      float: right;
      position: absolute;
      right: $padding;
      top: 0;
    }
    padding: $padding/2 $padding;
    background-color: $backgroundColor;
    text-align: center;
    font-size: 2rem;
    border-bottom: 1px solid #ccc;
  }

  #wrapper {
    padding: $padding * 2 $padding;
    background-color: $backgroundColor;
    margin-top: 48px + $padding;

    & > .row + .row {
      margin-top: $padding * 2;
    }

    & > .row {
      display: flex;
      flex-direction: row;
      & > .avatar > img {
        width: 72px;
        height: 72px;
        border-radius: $borderRadius;
      }
      & > .content {
        margin-left: $padding;
        max-width: 60vw;
        & > .nickname {
          margin: 0 0 8px 0;
          font-size: $fontSize;
        }
      }

      & ::before {
        font-size: $fontSize;
        color: #fff;
        position: absolute;
        left: -12px;
        top: 0;
      }
      & ::after {
        font-size: $fontSize;
        color: #fff;
        position: absolute;
        right: -12px;
        top: 0;
      }

      &.comment {
        &.self {
          flex-direction: row-reverse;
          & > .content {
            margin-left: 0;
            margin-right: $padding;
            & > .nickname {
              text-align: end;
            }
            & > .comment {
              background-color: $selfColor;
              &::before {
                content: none;
              }
              &::after {
                content: "\25B8";
                color: $selfColor;
              }
            }
          }
        }
        & > .content {
          & > .comment {
            &::before {
              content: "\25C2";
            }
            position: relative;
            border-radius: $borderRadius;
            padding: 16px;
            font-size: 1.4rem;
            background-color: #fff;
          }
        }
      }
      &.enter,
      &.like,
      &.follow {
        justify-content: center;
        color: #aaa;
        & > .nickname {
          color: $nicknameColor;
        }
      }
      &.gift {
        & > .content {
          & > .gift {
            &::before {
              content: "\25C2";
              color: $giftColor;
            }
            width: 300px;
            height: 60px;
            position: relative;
            border-radius: $borderRadius;
            padding: $padding;
            font-size: $fontSize;
            background-color: $giftColor;
            display: flex;
            flex-direction: row;
            align-items: center;
            color: $backgroundColor;
            & > .icon > img {
              height: 42px;
            }
            & > .detail {
              margin-left: 8px;
            }
          }
        }
      }
    }
  }
}
</style>
