<template>
  <el-row :gutter="20" class="container">
    <el-col :span="14">
      <!-- 基本信息 -->
      <el-row :gutter="12" style="padding: 0 10px 10px 10px">
        <el-col :span="8">
          <el-card shadow="always">
            <template #header>
              <div class="card-info-header">当前赛季</div>
            </template>
            <div class="card-content">
              <el-icon class="icon">
                <Medal/>
              </el-icon>
              {{ cardData.season }}
            </div>
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card shadow="always">
            <template #header>
              <div class="card-info-header">对战局数</div>
            </template>
            <div class="card-content">
              <el-icon class="icon">
                <GoldMedal/>
              </el-icon>
              胜: {{ cardData.winNum }}
              &nbsp;&nbsp;
              <el-icon class="icon">
                <Star/>
              </el-icon>
              负: {{ cardData.lostNum }}
            </div>
          </el-card>
        </el-col>
        <el-col :span="8">
          <el-card shadow="always">
            <template #header>
              <div class="card-info-header">我的积分</div>
            </template>
            <div class="card-content">
              <el-icon class="icon">
                <TrophyBase/>
              </el-icon>
              <span>{{ cardData.score }}</span>
            </div>
          </el-card>
        </el-col>
      </el-row>

      <!-- 创建房间 -->
      <el-row :gutter="12" class="room-row">
        <el-col
            :span="6"
            v-for="(item) in roomType"
        >
          <el-card shadow="always" class="room-card">
            <template #header>
              <div class="card-header">
                <el-icon><WindPower /></el-icon>
                {{ item.label }}
              </div>
            </template>
            <el-button
                type="primary"
                @click="createRoom(item.value)"
                style="height: 60px"
            >
              开始匹配
            </el-button>
          </el-card>
        </el-col>

        <RoomDialog ref="roomDialog"/>
      </el-row>
    </el-col>

    <!-- 排行榜 -->
    <el-col :span="10">
      <el-tabs
          v-model="activeRankTab"
          type="card"
      >
        <el-tab-pane
            v-for="(item) in rankTypes"
            :label="item.label"
            :name="item.value"
        >
          <RankBoard ref="rankBoard"/>
        </el-tab-pane>
      </el-tabs>
    </el-col>
  </el-row>
</template>

<script>
import RankBoard from "@/views/competition/components/rankBoard.vue";
import RoomDialog from "@/views/competition/components/roomDialog.vue";
import {CATALOG_ARRAY, ROOM_ARRAY} from "@/views/competition/const";

export default {
  inject: ['reload'],
  components: {
    RankBoard,
    RoomDialog
  },
  data() {
    return {
      roomType: ROOM_ARRAY,
      rankTypes: CATALOG_ARRAY,
      activeRankTab: 'CET4',
      cardData: {
        season: '第 1 赛季',
        score: '1000',
        winNum: 30,
        lostNum: 12
      }
    }
  },
  methods: {
    createRoom(type) {
      this.$refs.roomDialog.show(type);
    }
  }
}
</script>

<style scoped>
.container {
  width: 100%;
  height: 100%;
}

.room-row {
  padding: 10px;
  height: calc(100vh - 320px);
}

.room-card {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #f8eefc;
}

.card-info-header {
  color: #62aaf4;
  font-size: 16px;
  font-weight: bold;
  text-align: center;
}

.card-header {
  font-size: 16px;
  font-weight: bold;
  text-align: center;
}

.card-content {
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  font-size: 20px;
  margin-right: 4px;
}

::v-deep(.el-tabs__item) {
  font-weight: bold;
}
</style>
