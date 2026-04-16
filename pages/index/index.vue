<template>
  <view class="container">
    <!-- 1. 背景图层 (固定定位，实现视差滚动效果) -->
    <view class="bg-image" :style="{ backgroundImage: 'url(' + bgImage + ')' }"></view>
    
    <!-- 2. 顶部导航栏 (磨砂玻璃效果) -->
    <view class="navbar">
      <view class="nav-logo">Albert</view>
      <view class="nav-menu">
        <text v-for="(item, index) in navList" :key="index" class="nav-item" :class="{ active: index === 0 }">
          {{ item }}
        </text>
      </view>
      <view class="nav-right">
        <!-- 简单的月亮图标占位 -->
        <text class="icon-moon">🌙</text>
      </view>
    </view>

    <!-- 3. 主要内容区域 -->
    <view class="main-content">

      <!-- 第一部分：首屏介绍 -->
      <view class="section-hero">
        <view class="hero-text">
          <h1 class="title">你好, 我是 Albert</h1>
          <p class="subtitle">数学爱好者 & 开发者</p>
          <p class="desc">"If I never see you again, good morning, good afternoon and good night."</p>
          <view class="btn-group">
            <button class="btn primary">查看我的项目</button>
            <button class="btn outline">访问 GitHub</button>
          </view>
        </view>
        <view class="hero-image-wrapper">
          <!-- 头像图片 -->
          <image class="avatar" src="/static/logo.png" mode="aspectFill"></image>
        </view>
      </view>

      <!-- 第二部分：关于我 (左侧文字，右侧时间轴) -->
      <view class="section-about">
        <h2 class="section-title">关于我</h2>

        <view class="about-grid">
          <!-- 左侧简介 -->
          <view class="about-intro">
            <p>我目前是某高校数学与应用数学专业的在读大学生。</p>
            <p>这个网站是我记录学习、分享想法和展示成果的地方。</p>
            <p>热爱编程，喜欢折腾各种新技术。</p>
          </view>

          <!-- 右侧时间轴 -->
          <view class="timeline">
            <view class="timeline-item" v-for="(item, index) in timelineData" :key="index">
              <view class="timeline-dot"></view>
              <view class="timeline-content">
                <text class="t-date">{{ item.date }}</text>
                <text class="t-title">{{ item.title }}</text>
                <text class="t-desc">{{ item.desc }}</text>
              </view>
            </view>
          </view>
        </view>
      </view>

    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue';

// 背景图：这里使用了一个网络风景图，你可以替换为本地路径如 '/static/bg.jpg'
const bgImage = ref('https://images.unsplash.com/photo-1501785888041-af3ef285b470?ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80');

// 导航菜单
const navList = ['首页', '关于我', '项目', '我的频道', '技能', '联系我'];

// 时间轴数据
const timelineData = ref([
  { date: '2025.06 - 至今', title: '软件开发', desc: '发布自己的第一款软件“视频剪辑工具箱”' },
  { date: '2024.09 - 2025.05', title: '数学建模', desc: '获得全国大学生数学建模竞赛省级一等奖' },
  { date: '2023.09 - 2024.06', title: 'Web开发入门', desc: '系统学习Vue3与UniApp开发框架' }
]);
</script>

<style lang="scss" scoped>
/* 基础变量 */
$primary-color: #007AFF; /* iOS蓝色风格，可改为其他颜色 */
$text-dark: #333333;
$text-light: #666666;
$card-bg: rgba(255, 255, 255, 0.85); /* 磨砂白背景 */

.container {
  position: relative;
  min-height: 100vh;
  font-family: -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Helvetica, sans-serif;
  overflow-x: hidden;
}

/* 1. 背景图层 */
.bg-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  z-index: -1;
  /* 背景变暗一点，让文字更清晰 */
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.2);
  }
}

/* 2. 导航栏 */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 40px;
  box-sizing: border-box;
  z-index: 999;
  /* 磨砂玻璃效果 */
  background: rgba(255, 255, 255, 0.7);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);

  .nav-logo {
    font-weight: bold;
    font-size: 18px;
    color: $text-dark;
  }

  .nav-menu {
    display: flex;
    gap: 20px;
    /* 在移动端隐藏部分菜单或改为汉堡菜单，这里简单处理 */
    @media (max-width: 768px) {
      display: none;
    }
  }

  .nav-item {
    font-size: 14px;
    color: $text-light;
    cursor: pointer;
    transition: color 0.3s;
    &.active, &:hover {
      color: $primary-color;
      font-weight: 500;
    }
  }

  .nav-right {
    cursor: pointer;
    color: $text-dark;
  }
}

/* 3. 主内容区 */
.main-content {
  max-width: 1000px;
  margin: 0 auto;
  padding: 100px 20px 40px; /* 顶部留出导航栏空间 */

  /* 公共卡片风格 */
  .section-card {
    background: $card-bg;
    border-radius: 16px;
    padding: 30px;
    margin-bottom: 30px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.05);
    backdrop-filter: blur(5px);
  }
}

/* 首屏样式 */
.section-hero {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 60px 0;
  color: white; /* 首屏文字为白色 */
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);

  @media (max-width: 768px) {
    flex-direction: column-reverse; /* 移动端头像在上 */
    text-align: center;
    padding-top: 20px;
  }

  .hero-text {
    flex: 1;
    padding-right: 40px;
    @media (max-width: 768px) {
      padding-right: 0;
      margin-top: 30px;
    }

    .title { font-size: 36px; margin-bottom: 10px; font-weight: bold; }
    .subtitle { font-size: 20px; margin-bottom: 20px; opacity: 0.9; }
    .desc { font-size: 14px; line-height: 1.6; margin-bottom: 30px; opacity: 0.8; }
  }

  .hero-image-wrapper {
    flex-shrink: 0;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    border: 5px solid rgba(255,255,255,0.3);
    overflow: hidden;
    box-shadow: 0 8px 30px rgba(0,0,0,0.2);
    background: white;

    .avatar {
      width: 100%;
      height: 100%;
    }
  }

  .btn-group {
    display: flex;
    gap: 15px;
    @media (max-width: 768px) {
      justify-content: center;
    }

    .btn {
      padding: 10px 24px;
      border-radius: 30px;
      font-size: 14px;
      font-weight: 500;
      cursor: pointer;
      transition: transform 0.2s;
      border: none;
      outline: none;

      &:active { transform: scale(0.95); }
    }

    .primary {
      background: $primary-color;
      color: white;
    }

    .outline {
      background: rgba(255,255,255,0.2);
      color: white;
      border: 1px solid rgba(255,255,255,0.5);
    }
  }
}

/* 关于我区域 */
.section-about {
  /* 这里使用白色背景卡片，模拟截图下半部分 */
  background: #f8f9fa;
  padding: 40px;
  border-radius: 20px;
  color: $text-dark;
  margin-top: 20px;

  @media (max-width: 768px) {
    padding: 20px;
    background: rgba(255,255,255,0.9); /* 移动端稍微透明一点 */
  }

  .section-title {
    font-size: 24px;
    margin-bottom: 30px;
    text-align: center;
    position: relative;
    &::after {
      content: '';
      display: block;
      width: 50px;
      height: 3px;
      background: $primary-color;
      margin: 10px auto 0;
      border-radius: 2px;
    }
  }

  .about-grid {
    display: flex;
    gap: 40px;
    @media (max-width: 768px) {
      flex-direction: column;
    }
  }

  .about-intro {
    flex: 1;
    line-height: 1.8;
    color: $text-light;
    p { margin-bottom: 15px; }
  }

  /* 时间轴样式 */
  .timeline {
    flex: 1;
    border-left: 2px solid #e0e0e0;
    margin-left: 10px;
    padding-left: 20px;

    .timeline-item {
      position: relative;
      margin-bottom: 30px;
      padding-top: 5px;

      /* 时间轴圆点 */
      .timeline-dot {
        position: absolute;
        left: -29px;
        top: 8px;
        width: 12px;
        height: 12px;
        border-radius: 50%;
        background: white;
        border: 3px solid $primary-color;
        box-sizing: border-box;
      }

      .t-date {
        display: block;
        font-size: 12px;
        color: #999;
        margin-bottom: 4px;
      }
      .t-title {
        display: block;
        font-weight: bold;
        font-size: 16px;
        color: $text-dark;
      }
      .t-desc {
        display: block;
        font-size: 14px;
        color: $text-light;
        margin-top: 4px;
      }
    }
  }
}
</style>