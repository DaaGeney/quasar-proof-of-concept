<template>
  <q-page class="relative-position">
     <q-scroll-area class="absolute fullscreen">
        <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
          <div class="col">
            <q-input
              class="new-qweet"
              bottom-slots
              v-model="newQweetContent"
              label="What's happening?"
              counter
              maxlength="128"
              autogrow
            >
              <template v-slot:before>
                <q-avatar size="xl">
                  <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
                </q-avatar>
              </template>
            </q-input>
          </div>
          <div class="col col-shrink">
            <q-btn
              @click="addNewQweet()"
              rounded
              class="q-mb-lg"
              :disable="!newQweetContent"
              color="primary"
              label="Qweet"
              unelevated
            />
          </div>
        </div>
        <q-separator class="divider" size="10px" color="grey-2" />

        <q-list separator>
          <transition-group
            appear
            enter-active-class="animated fadeIn"
            leave-active-class="animated fadeOut"
          >
            <q-item
              class="qweet q-py-md"
              v-for="qweet in data.qweets"
              :key="qweet.date"
            >
              <q-item-section avatar top>
                <q-avatar size="xl">
                  <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
                </q-avatar>
              </q-item-section>

              <q-item-section>
                <q-item-label >
                  <strong> Diego Assia</strong>
                  <span class="text-grey-7">
                    @dassia
                    <br class="lt-md"> &bull; {{ relativeDate(qweet.date) }} <br/>
                  </span>
                  </q-item-label>
                <q-item-label class="qweet-content text-body1">
                  {{ qweet.content }}
                </q-item-label>

                <div class="qweet-icons row justify-between q-mt-sm">
                  <q-btn
                    flat
                    size="sm"
                    round
                    color="grey"
                    icon="far fa-comment"
                  />
                  <q-btn
                    flat
                    size="sm"
                    round
                    color="grey"
                    icon="fas fa-retweet"
                  />
                  <q-btn
                    flat
                    size="sm"
                    round
                    color="grey"
                    icon="far fa-heart"
                  />
                  <q-btn
                  @click="deleteQweet(qweet)"
                    flat
                    size="sm"
                    round
                    color="grey"
                    icon="fas fa-trash"
                  />
                </div>
              </q-item-section>
            </q-item>
          </transition-group>
        </q-list>
        </q-scroll-area>
  </q-page>
</template>

<script lang="ts">
import { formatDistance } from 'date-fns';
import { defineComponent, ref, reactive } from 'vue';
import { Qweet } from './models';

export default defineComponent({
  name: 'PageHome',
  setup() {
    const newQweetContent = ref('');
    const data = reactive({
      qweets: [
        {
          content: 'Lorem ipsum dolor sit amet consectetur  adipisicing elit. Aliquid libero provident unde tempore architecto expedita pariatur totam similique quod',
          date: 1643840208668,
        },
        {
          content: 'Lorem ipsum dolor sit amet consectetur  adipisicing elit. Aliquid libero provident unde tempore architecto expedita pariatur totam similique quod',
          date: 1643840213985,
        },
      ],
    });

    const relativeDate = (value: number) => formatDistance(value, new Date());

    const addNewQweet = () => {
      data.qweets.unshift({
        content: newQweetContent.value,
        date: Date.now(),
      });
      newQweetContent.value = '';
    };

    const deleteQweet = (qweetToDelete: Qweet) => {
      data.qweets = data.qweets.filter((qweet) => qweet.date !== qweetToDelete.date);
    };

    return {
      newQweetContent,
      data,
      relativeDate,
      addNewQweet,
      deleteQweet,
    };
  },
});
</script>

<style lang="sass">
.new-qweet
  textarea
    font-size: 19px
    line-height: 1 !important

.divider
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4

.qweet:not(:first-child)
  border-top: 1px solid rgba(0, 0, 0, 0.12)

.qweet-content
  white-space: pre-line

.qweet-icons
  margin-left: -5px
</style>
