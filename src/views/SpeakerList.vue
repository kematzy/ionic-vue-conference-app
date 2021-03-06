<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar color="primary">
        <ion-buttons slot="start">
          <ion-menu-button></ion-menu-button>
        </ion-buttons>
        <ion-title>Speakers</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="outer-content">
      <ion-list>
        <ion-grid fixed>
          <ion-row align-items-stretch>
            <ion-col size="12" size-md="6" v-for="speaker in speakers" :key="speaker.id">
              <ion-card class="speaker-card">
                <ion-card-header>
                  <ion-item detail="false" lines="none" button :href="'app/tabs/(speakers:speaker-details/' + speaker.id">
                    <ion-avatar slot="start">
                      <img v-bind:src="speaker.profilePic" alt="Speaker profile pic" />
                    </ion-avatar>
                    {{speaker.name}}
                  </ion-item>
                </ion-card-header>

                <ion-card-content>
                  <ion-list>
                    <ion-item v-for="session in sessionsBySpeaker(speaker.id)" button @click="goToSessionDetail(session)" :key="session.id">
                      <h3>{{session.name}}</h3>
                    </ion-item>

                    <ion-item button @click="goToSpeakerDetail(speaker)">
                      <h3>About {{speaker.name}}</h3>
                    </ion-item>
                  </ion-list>
                </ion-card-content>

                <ion-row no-padding justify-content-center>
                  <ion-col text-left size="4">
                    <ion-button fill="clear" size="small" color="primary" @click="gotToOffsite('Tweet')">
                      <ion-icon name="logo-twitter" slot="start"></ion-icon>
                      Tweet
                    </ion-button>
                  </ion-col>
                  <ion-col text-center size="4">
                    <ion-button fill="clear" size="small" color="primary" @click="gotToOffsite('Share')">
                      <ion-icon name='share-alt' slot="start"></ion-icon>
                      Share
                    </ion-button>
                  </ion-col>
                  <ion-col text-right size="4">
                    <ion-button fill="clear" size="small" color="primary" @click="gotToOffsite('Contact')">
                      <ion-icon name='chatboxes' slot="start"></ion-icon>
                      Contact
                    </ion-button>
                  </ion-col>
                </ion-row>
              </ion-card>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-list>
    </ion-content>
  </div>
</template>

<style scoped>
  .map-canvas {
    position: absolute;
    height: 100%;
    width: 100%;
    background-color: transparent;
    opacity: 0;
    transition: opacity 150ms ease-in
  }

  .show-map {
    opacity: 1;
  }
</style>

<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';
  import { mapGetters } from 'vuex';
  import { Speaker } from '@/store/modules/speakers';
  import { Session} from '@/store/modules/sessions';

  @Component
  export default class SpeakerList extends Vue {
    get speakers() {
      return this.$store.state.speakers.speakers.concat().sort();
    }

    sessionsBySpeaker(speakerId: number) {
      return this.$store.state.sessions.sessions
        .filter((s: Session) => s.speakerIds.indexOf(speakerId) !== -1);
    }

    mounted() {
      this.$store.dispatch('loadSessionData');
      this.$store.dispatch('loadSpeakerData');
    }
    goToSessionDetail(session: Session) {
      // this.$router.push({ name: 'session-detail', params: { sessionId: session.id.toString() } });
    }
    goToSpeakerDetail(speaker: Speaker) {
      // this.$router.push({ name: 'speaker-detail', params: { sessionId: speaker.id.toString() } });
    }
    async gotToOffsite(msg: string) {
      const loading = await this.$ionic.loadingController.create({
        message: msg,
        duration: (Math.random() * 1000) + 500
      });
      await loading.present();
      await loading.onWillDismiss();
    }
  }
</script>
