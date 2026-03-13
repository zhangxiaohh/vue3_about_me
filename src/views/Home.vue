<template>
  <div class="home-layout">
    <!-- 左侧个人资料侧边栏 -->
    <aside class="profile-sidebar">
      <div class="profile-header">
        <div class="avatar-wrapper">
          <img src="../assets/image.png" alt="Avatar" class="avatar" />
          <span class="badge">张</span>
        </div>
        <h1 class="profile-name">小张的个人简历</h1>
        <p class="tagline">其实，我的愿望是世界和平！</p>
      </div>

      <div class="divider"></div>

      <div class="social-links-container">
        <div class="social-links">
          <a @click.prevent="toggleContact('github')" href="#" class="social-icon" :class="{ active: activeContact === 'github' }">
            <Github :size="25" />
          </a>
          <a @click.prevent="toggleContact('mail')" href="#" class="social-icon" :class="{ active: activeContact === 'mail' }">
            <Mail :size="25" />
          </a>
          <a @click.prevent="toggleContact('wechat')" href="#" class="social-icon" :class="{ active: activeContact === 'wechat' }">
            <MessageCircle :size="25" />
          </a>
          <a @click.prevent="toggleContact('phone')" href="#" class="social-icon" :class="{ active: activeContact === 'phone' }">
            <Phone :size="25" />
          </a>
        </div>
        
        <!-- 联系方式悬浮窗 -->
        <Transition name="fade-slide">
          <div v-if="activeContact" class="contact-popover" @click="copyToClipboard">
            <span class="contact-label">{{ contactLabel }}:</span>
            <span class="contact-value">{{ contactValue }}</span>
            <span class="copy-hint" v-if="!copied">点击复制</span>
            <span class="copy-hint success" v-else>已复制!</span>
            <div class="popover-arrow"></div>
          </div>
        </Transition>
      </div>

      <div class="divider"></div>

      <ul class="personal-info">
        <li>
          <span class="label">毕业院校</span>
          <span class="value">河南科技学院</span>
        </li>
        <li>
          <span class="label">所在城市</span>
          <span class="value">河南 新乡</span>
        </li>
        <li>
          <span class="label">年级</span>
          <span class="value">27届毕业生</span>
        </li>
      </ul>

      <div class="divider"></div>

      <button class="contact-button" @click="router.push('/contact')">联系我</button>
    </aside>

    <!-- 右侧内容区域 -->
    <main class="content-area">
      <header class="section-header">
        <h2 class="section-title">我的能力</h2>
        <div class="dotted-line"></div>
      </header>

      <div class="skills-grid">
        <!-- UI设计 -->
        <div class="skill-card">
          <div class="card-icon">☁️</div>
          <h3 class="card-title">UI设计</h3>
          <ul class="card-content">
            <li>UI排版</li>
            <li>响应式页面</li>
            <li>静态页面、动画效果</li>
          </ul>
          <a href="#" class="order-link">ORDER NOW</a>
        </div>

        <!-- 网页制作 -->
        <div class="skill-card">
          <div class="card-icon">💻</div>
          <h3 class="card-title">网页制作</h3>
          <ul class="card-content">
            <li>HTML、JS、CSS、SCSS</li>
            <li>JS交互</li>
            <li>AJAX数据渲染</li>
          </ul>
          <a href="#" class="order-link">ORDER NOW</a>
        </div>

        <!-- 前端 -->
        <div class="skill-card">
          <div class="card-icon">⚛️</div>
          <h3 class="card-title">前端</h3>
          <ul class="card-content">
            <li>Vue框架</li>
            <li>React框架</li>
            <li>HTML CSS JS</li>
          </ul>
          <a href="#" class="order-link">ORDER NOW</a>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
  import { ref, computed } from 'vue'
  import { useRouter } from 'vue-router'
  import { Github, Mail, MessageCircle, Phone } from 'lucide-vue-next'

  const router = useRouter()

  // 联系方式状态
  const activeContact = ref<string | null>(null)
  const copied = ref(false)

  const contacts = {
    github: { label: 'GitHub', value: 'https://github.com/zhangxiaohh' },
    mail: { label: '邮箱', value: '210567852@qq.com' },
    wechat: { label: '微信', value: 'vento040624' },
    phone: { label: '电话', value: '15138140100' }
  }

  const contactLabel = computed(() => activeContact.value ? contacts[activeContact.value as keyof typeof contacts].label : '')
  const contactValue = computed(() => activeContact.value ? contacts[activeContact.value as keyof typeof contacts].value : '')

  const toggleContact = (type: string) => {
    if (activeContact.value === type) {
      activeContact.value = null
    } else {
      activeContact.value = type
      copied.value = false
    }
  }

  const copyToClipboard = async () => {
    if (!contactValue.value) return
    try {
      await navigator.clipboard.writeText(contactValue.value)
      copied.value = true
      setTimeout(() => {
        copied.value = false
      }, 2000)
    } catch (err) {
      console.error('复制失败:', err)
    }
  }
</script>

<style scoped>
.home-layout {
  display: flex;
  gap: 2rem;
  max-width: 1400px;
  margin: 0 auto;
  padding: 1rem;
  height: 100%; /* 使用父容器的 100% 高度 */
  overflow: hidden;
}

/* 侧边栏样式 */
.profile-sidebar {
  width: 320px;
  background-color: var(--sidebar-bg);
  padding: 2.5rem 2rem;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  flex-shrink: 0;
  height: 100%; /* 撑满布局高度 */
}

.avatar-wrapper {
  position: relative;
  margin-bottom: 1.5rem;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  border: 4px solid rgba(255, 255, 255, 0.1);
  background: #fff;
}

.badge {
  position: absolute;
  bottom: 0;
  right: 0;
  background-color: var(--accent-color);
  color: white;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  border: 2px solid var(--sidebar-bg);
}

.profile-name {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
  font-weight: 600;
}

.tagline {
  color: var(--text-secondary);
  font-size: 0.9rem;
  margin-bottom: 1.5rem;
}

.divider {
  width: 100%;
  height: 1px;
  border-top: 1px dotted rgba(255, 255, 255, 0.2);
  margin: 1.5rem 0;
}

.social-links-container {
  position: relative;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.social-links {
  display: flex;
  gap: 1.5rem;
  font-size: 1.2rem;
  z-index: 2;
}

.social-icon {
  text-decoration: none;
  opacity: 0.7;
  transition: all 0.3s ease;
  color: var(--text-main);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 8px;
  border-radius: 50%;
}

.social-icon:hover, .social-icon.active {
  opacity: 1;
  background-color: rgba(255, 255, 255, 0.1);
  color: var(--accent-color);
  transform: translateY(-2px);
}

/* 联系方式悬浮窗样式 */
.contact-popover {
  position: absolute;
  bottom: 100%; /* 显示在图标上方 */
  margin-bottom: 15px;
  background: var(--card-bg);
  padding: 12px 20px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 200px;
  cursor: pointer;
  z-index: 10;
}

.contact-label {
  font-size: 0.75rem;
  color: var(--accent-color);
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 4px;
}

.contact-value {
  font-size: 0.95rem;
  font-weight: 600;
  color: #fff;
  word-break: break-all;
  text-align: center;
}

.copy-hint {
  margin-top: 8px;
  font-size: 0.7rem;
  color: var(--text-secondary);
  background: rgba(255, 255, 255, 0.05);
  padding: 2px 8px;
  border-radius: 4px;
}

.copy-hint.success {
  color: #4ade80;
  background: rgba(74, 222, 128, 0.1);
}

.popover-arrow {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  border-left: 8px solid transparent;
  border-right: 8px solid transparent;
  border-top: 8px solid var(--card-bg);
}

/* 动画效果 */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(10px) scale(0.9);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(10px) scale(0.9);
}

.personal-info {
  width: 100%;
  list-style: none;
  padding: 0;
}

.personal-info li {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.8rem;
  font-size: 0.9rem;
}

.personal-info .label {
  color: var(--text-main);
  opacity: 0.8;
}

.personal-info .value {
  color: var(--text-secondary);
}

.contact-button {
  width: 100%;
  padding: 1rem;
  background-color: var(--accent-color);
  border: none;
  border-radius: 30px;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: transform 0.2s, opacity 0.2s;
  margin-top: auto;
}

.contact-button:hover {
  opacity: 0.9;
  transform: translateY(-2px);
}

/* 内容区域样式 */
.content-area {
  flex: 1;
  padding: 1rem 0;
  height: 100%;
  overflow-y: auto; /* 开启垂直滚动条 */
  padding-right: 1rem; /* 为滚动条预留空间 */
}

/* 滚动条美化 (Webkit) */
.content-area::-webkit-scrollbar {
  width: 6px;
}
.content-area::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
}
.content-area::-webkit-scrollbar-thumb {
  background: var(--accent-color);
  border-radius: 10px;
}
.content-area::-webkit-scrollbar-thumb:hover {
  background: #ff8585;
}

.section-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 3rem;
}

.section-title {
  font-size: 1.5rem;
  font-weight: 600;
  white-space: nowrap;
}

.dotted-line {
  flex: 1;
  height: 1px;
  border-top: 1px dotted rgba(255, 255, 255, 0.3);
}

.skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 2rem;
}

.skill-card {
  background-color: var(--card-bg);
  padding: 2.5rem 2rem;
  border-radius: 12px;
  transition: transform 0.3s;
}

.skill-card:hover {
  transform: translateY(-5px);
}

.card-icon {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
  color: var(--accent-color);
}

.card-title {
  font-size: 1.25rem;
  margin-bottom: 1.5rem;
  font-weight: 600;
}

.card-content {
  list-style: none;
  padding: 0;
  margin-bottom: 2rem;
}

.card-content li {
  color: var(--text-secondary);
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.order-link {
  color: var(--accent-color);
  text-decoration: none;
  font-weight: 700;
  font-size: 0.9rem;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.order-link:hover {
  text-decoration: underline;
}

/* 响应式调整 */
@media (max-width: 992px) {
  .home-layout {
    flex-direction: column;
    align-items: center;
  }
  
  .profile-sidebar {
    width: 100%;
    max-width: 500px;
  }
}
</style>
