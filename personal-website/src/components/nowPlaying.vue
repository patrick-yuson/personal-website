<script setup>
import headerElement from './headerElement.vue'
import albumCoverElement from './albumCoverElement.vue'
import cardElement from './cardElement.vue'
import iconElement from './iconElement.vue'
import { useSongEventsHandler } from '@/composables/songEventsHandler'
import { useRedirect } from '@/composables/useRedirect'

const { currentSong } = useSongEventsHandler()
const { redirectToUrl } = useRedirect()
</script>

<template>
  <div class="now-playing-main component">
    <headerElement name="Now Playing" size="h3" weight="bold" />
    <albumCoverElement
      v-if="currentSong.album_cover"
      class="now-playing-album"
      :src="currentSong.album_cover"
    />
    <div class="now-playing-title">{{ currentSong.title }}</div>
    <div class="now-playing-artist">{{ currentSong.artist }}</div>
    <div v-for="item in currentSong.details" :key="item.id">
      <cardElement v-if="item.type === 'card'" :header="item.header" :headerImg="item.header_src">
        <template #content>
          <div v-if="item.subType === 'plaintext'">
            {{ item.description }}
          </div>
          <div class="icon-array" v-else-if="item.subType === 'iconArray'">
            <div class="icon-row">
              <div v-for="icon in item.icons" :key="icon.id" class="icon-wrapper">
                <div class="icon-and-caption">
                  <iconElement :src="icon.src" :height="'60px'" :width="'60px'" />
                  <div class="caption">{{ icon.name }}</div>
                </div>
              </div>
            </div>
          </div>
          <div v-else-if="item.subType === 'header-photo-plaintext'">
            <div :class="{ 'space-bottom': item.website !== null }">{{ item.description }}</div>
            <div class="url" @click="redirectToUrl(item.website)">Check out their website!</div>
          </div>
        </template>
      </cardElement>
    </div>
  </div>
</template>

<style>
.now-playing-main {
  height: 100vh;
}
.now-playing-title {
  margin: 20px 0px 0px 0px;
  font-size: var(--font-size-h2);
  font-weight: bold;
}
.now-playing-artist {
  margin: 5px 0px 0px 0px;
  font-size: 18px;
}
.now-playing-album {
  height: 318px;
  width: 318px;
}
.icon-wrapper {
  height: 60px;
  width: 60px;
}
.icon-row {
  display: flex;
  flex-direction: row;
  gap: 32px;
  overflow-x: scroll;
  min-height: 115px;
  padding: 5px;
}
.icon-and-caption {
  display: flex;
  flex-direction: column;
  gap: 5px;
  font-size: var(--font-size-caption);
  width: 100%;
  align-items: center;
}
.caption {
  text-align: center;
}
.url {
  color: var(--hh-c-text-secondary);
}
.url:hover {
  cursor: pointer;
  text-decoration: underline;
}
.space-bottom {
  margin-bottom: 20px;
}
</style>
