<template>
  <div class="page">
    <div class="bg-glow bg-glow--one"></div>
    <div class="bg-glow bg-glow--two"></div>

    <button v-if="!success && !burning" class="close-btn" type="button" aria-label="关闭" @click="reset">
      ×
    </button>

    <main v-if="!success && !burning" class="sheet">
      <section class="panel">
        <div class="field-block">
          <label class="section-label">烧钱金额</label>
          <div class="amount-box">
            <input v-model.number="amount" type="number" min="1" />
            <span>亿</span>
          </div>

          <div class="quick-grid">
            <button
              v-for="item in quickAmounts"
              :key="item"
              class="quick-btn"
              :class="{ active: amount === item }"
              @click="amount = item"
            >
              {{ item }}亿
            </button>
          </div>
        </div>

        <div class="field-block">
          <label class="section-label">收款人</label>
          <input
            v-model="recipient"
            class="recipient-box"
            type="text"
            placeholder="请输入收款人姓名"
          />
        </div>

        <div class="field-block">
          <label class="section-label">纸钞样式</label>
          <div class="select-box">
            <div class="select-left">
              <span class="money-icon">💵</span>
              <span>{{ billStyle }}</span>
            </div>
            <span class="chevron">⌄</span>
          </div>
        </div>

        <div class="field-block">
          <label class="section-label">转账留言</label>
          <textarea
            v-model="message"
            class="message-box"
            placeholder="想对收款人说什么..."
            rows="5"
          ></textarea>
        </div>

        <div class="field-block">
          <label class="section-label">祝福语</label>
          <div class="wish-grid">
            <button
              v-for="item in wishes"
              :key="item"
              class="wish-btn"
              :class="{ active: blessing === item }"
              @click="blessing = item"
            >
              {{ item }}
            </button>
          </div>

          <textarea class="message-box selected-wish" rows="2" readonly :value="blessing"></textarea>
        </div>

        <button class="confirm-btn" @click="startBurn">✦ 开始烧钱 ✈</button>
      </section>
    </main>

    <main v-else-if="burning" class="burning-screen">
      <section class="burning-card">
        <div class="burning-orb">
          <div class="burning-orb__inner"></div>
          <div class="burning-flame"></div>
          <div class="burning-spark spark-1"></div>
          <div class="burning-spark spark-2"></div>
          <div class="burning-spark spark-3"></div>
          <div class="burning-spark spark-4"></div>
        </div>

        <h1>烧钱中...</h1>
        <p class="subtitle">正在将你的心意送达阴间</p>

        <div class="status-line">
          <span>✦</span>
          <span>焚烧 {{ amount }} 亿 {{ billStyle }}</span>
          <span>✦</span>
        </div>

        <p class="recipient-line">收款人：{{ recipient || '未填写' }}</p>
        <p class="recipient-line">留言：{{ message || '未填写留言' }}</p>
        <p class="recipient-line">祝福语：{{ blessing }}</p>
      </section>
    </main>

    <main v-else class="success-screen">
      <section class="success-card">
        <div class="success-badge">
          <span>✓</span>
        </div>
        <h1>烧钱成功！</h1>
        <p class="subtitle">已成功向 {{ recipient || '未填写' }} 烧钱</p>

        <div class="success-detail">
          <span>+{{ amount }}亿 {{ billStyle }}</span>
        </div>
        <div class="success-detail success-detail--muted">
          <span>{{ message || '未填写留言' }}</span>
        </div>

        <button class="confirm-btn" @click="reset">再来一次</button>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const amount = ref(100)
const recipient = ref('')
const billStyle = ref('冥通银行券')
const message = ref('')
const blessing = ref('愿你在阴间遇得比阳间好')
const success = ref(false)
const burning = ref(false)

const quickAmounts = [100, 500, 1000, 5000]
const wishes = [
  '愿你在阴间遇得比阳间好',
  '希望你能收到这份心意',
  '祝你阴间大发财',
  '愿祖先保佑你平安',
  '希望能帮到你的忙',
  '祝你好运连连',
  '阴间见财起意',
  '冥福绵绵无绝期',
]

function startBurn() {
  burning.value = true
  success.value = false
  setTimeout(() => {
    burning.value = false
    success.value = true
  }, 2200)
}

function reset() {
  amount.value = 100
  recipient.value = ''
  billStyle.value = '冥通银行券'
  message.value = ''
  blessing.value = '愿你在阴间遇得比阴间好'
  success.value = false
  burning.value = false
}
</script>
