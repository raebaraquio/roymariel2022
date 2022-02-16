<template>
  <q-page>
    <div>
      <div class="text-h4 q-my-lg text-dbg">Replies</div>
      <q-tab-panels
          class="text-dbg"
          v-model="tabName"
          animated
          swipeable
          transition-prev="jump-up"
          transition-next="jump-up"
          @update:model-value="tabChanged">
          <q-tab-panel name="rsvp">
            <!-- For non-small devices -->
            <div class="row q-mb-sm gt-xs">
              <q-input
                outlined
                dense
                clearable
                v-model="keyword"
                :debounce="400"
                placeholder="Search"
                class="q-py-none"
              style="max-width:300px"
              />
              <q-separator vertical inset  class="q-mx-sm"/>
              <q-chip clickable
                :outline="replyFilter !== 'GOING'"
                color="primary"
                text-color="white"
                @click="filterBy('GOING')">
                Going ({{going}})
              </q-chip>
              <q-chip clickable
                :outline="replyFilter !== 'NOT GOING'"
                color="accent"
                text-color="white"
                @click="filterBy('NOT GOING')">
                Not Going ({{notGoing}})
              </q-chip>
            </div>

            <!-- For small devices -->
            <div class="q-mb-sm lt-sm">
              <div>
                <q-input
                  outlined
                  dense
                  v-model="keyword"
                  :debounce="400"
                clearable
                  placeholder="Search"
                  class="q-py-none"
                  @update:model-value="$emit('search')"
                  />
              </div>
              <q-separator class="q-my-sm"/>
              <div>
                <q-chip clickable
                  :outline="replyFilter !== 'GOING'"
                  color="primary"
                  text-color="white"
                  @click="filterBy('GOING')">
                  Going ({{going}})
                </q-chip>
                <q-chip clickable
                  :outline="replyFilter !== 'NOT GOING'"
                  color="accent"
                  text-color="white"
                  @click="filterBy('NOT GOING')">
                  Not Going ({{notGoing}})
                </q-chip>
              </div>
            </div>
            <q-table
              title="RSVP"
              class="text-dbg"
              :rows="rows"
              :columns="columns"
              :loading="isLoading"
              :grid="$q.screen.lt.md"
              row-key="name"
              style="height:60vh"
            >
              <template v-slot:top-right>
                <q-btn
                  round
                  color="primary"
                  text-color="white"
                  icon="archive"
                  no-caps
                  @click="exportTable('rsvp')"
                />
              </template>
            </q-table>
          </q-tab-panel>
          <q-tab-panel name="playlist">
            <q-input
              outlined
              dense
              v-model="keyword"
                clearable
              :debounce="400"
              placeholder="Search"
              class="lt-sm q-py-none q-mb-sm"
              />
            <q-input
              outlined
              dense
              v-model="keyword"
              :debounce="400"
                clearable
              placeholder="Search"
              class="gt-xs q-py-none q-mb-sm"
              style="max-width:300px"/>
            <q-table
              title="Playlist"
              class="text-dbg"
              :rows="prows"
              :columns="pcolumns"
              :loading="isLoading"
              :grid="$q.screen.lt.md"
              row-key="name"
              style="height:60vh"
            >
              <template v-slot:top-right>
                <q-btn
                  round
                  color="primary"
                  text-color="white"
                  icon="archive"
                  no-caps
                  @click="exportTable('playlist')"
                />
              </template>
            </q-table>
          </q-tab-panel>
      </q-tab-panels>
      <q-separator />
      <q-footer>
        <q-tabs
          v-model="tabName"
          no-caps
          text-color="secondary"
          active-color="white"
          indicator-color="white"
          align="justify"
          class="shadow-2"
          @update:model-value="tabChanged">
          <q-route-tab
            icon="home"
            to="/"
            exact
          />
          <q-tab name="rsvp" label="RSVP" />
          <q-tab name="playlist" label="Playlist" />
        </q-tabs>
      </q-footer>
    </div>
  </q-page>
</template>
<script>
import { defineComponent, onMounted, computed, ref } from 'vue'
import { useClient } from '@vueauth/supabase'
import { exportFile, useQuasar } from 'quasar'

function wrapCsvValue (val, formatFn) {
  let formatted = formatFn !== void 0
    ? formatFn(val)
    : val

  formatted = formatted === void 0 || formatted === null
    ? ''
    : String(formatted)

  formatted = formatted.split('"').join('""')
  /**
   * Excel accepts \n and \r in strings, but some other CSV parsers do not
   * Uncomment the next two lines to escape new lines
   */
  // .split('\n').join('\\n')
  // .split('\r').join('\\r')

  return `"${formatted}"`
}

export default defineComponent({
  name: 'Replies',

  setup () {
    const $q = useQuasar()
    const supabase = useClient()
    const tabName = ref('rsvp')
    const isLoading = ref(false)
    const rows = ref([])
    const keyword = ref('')
    const replyFilter = ref('')

    const prows = ref([])

    const columns = [
      {
        name: 'last_name',
        label: 'Last Name',
        align: 'left',
        field: 'last_name',
        sortable: true
      },
      {
        name: 'first_name',
        label: 'First Name',
        align: 'left',
        field: 'first_name',
        sortable: true
      },
      {
        name: 'is_going',
        label: 'Reply',
        align: 'left',
        field: 'is_going',
        sortable: true
      },
      {
        name: 'mobile_number',
        label: 'Mobile',
        align: 'left',
        field: 'mobile_number',
        sortable: true
      },
      {
        name: 'updated_at',
        label: 'Update at',
        align: 'left',
        field: 'updated_at',
        sortable: true
      }
    ]
    const pcolumns = [
      {
        name: 'song',
        label: 'Song/s',
        align: 'left',
        field: 'song',
        sortable: true
      },
      {
        name: 'artist',
        label: 'Artist/s',
        align: 'left',
        field: 'artist',
        sortable: true
      }
      // {
      //   name: 'created_at',
      //   label: 'Created at',
      //   align: 'left',
      //   field: 'created_at',
      //   sortable: true
      // }
    ]

    const getData = async () => {
      /** Check if mobile number is existing */
      isLoading.value = true

      const response = await supabase
        .from('rsvp')
        .select('mobile_number, first_name, last_name, is_going, updated_at')

      isLoading.value = false
      if (response.error) {
        $q.notify({
          message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${response.statusText})`,
          multiLine: true,
          color: 'negative',
          timeout: 2300
        })
        return false
      }

      rows.value.splice(0, rows.value.length, ...response.data)
    }

    const getSongs = async () => {
      /** Check if mobile number is existing */
      isLoading.value = true

      const response = await supabase
        .from('playlist')
        .select('song, artist, created_at')

      isLoading.value = false
      if (response.error) {
        $q.notify({
          message: `Ooops! An error occurred while processing your reply. Please try again. (Error: ${response.statusText})`,
          multiLine: true,
          color: 'negative',
          timeout: 2300
        })
        return false
      }

      prows.value.splice(0, prows.value.length, ...response.data)
    }

    onMounted(() => {
      refreshData()
    })

    function refreshData () {
      getData()
      getSongs()
    }

    const filterBy = (reply) => {
      if (replyFilter.value === reply) {
        replyFilter.value = ''
      } else {
        replyFilter.value = reply
      }
    }

    const filteredReplies = computed(() => {
      let returnData = rows.value
      if (keyword.value && keyword.value.trim().length > 0) {
        const regExp = new RegExp(keyword.value, 'gi')
        returnData = rows.value.filter(a => {
          return (
            a.first_name.match(regExp) ||
            a.last_name.match(regExp) ||
            a.mobile_number.match(regExp) ||
            a.is_going.match(regExp)
          )
        })
      }
      if (replyFilter.value !== '') {
        returnData = returnData.filter(a => a.is_going === replyFilter.value)
      }
      return returnData
    })

    const going = computed(() => {
      return rows.value.filter(a => a.is_going === 'GOING').length
    })

    const notGoing = computed(() => {
      return rows.value.filter(a => a.is_going === 'NOT GOING').length
    })

    const filteredSongs = computed(() => {
      let returnData = prows.value
      if (keyword.value && keyword.value.trim().length > 0) {
        const regExp = new RegExp(keyword.value, 'gi')
        returnData = prows.value.filter(a => {
          return (
            a.song.match(regExp) ||
            a.artist.match(regExp)
          )
        })
      }
      return returnData
    })

    const exportTable = (table) => {
      let useRows = prows.value
      let useCols = pcolumns
      if (table === 'rsvp') {
        useRows = rows.value
        useCols = columns
      }
      // naive encoding to csv format
      const content = [useCols.map(col => wrapCsvValue(col.label))].concat(
        useRows.map(row => useCols.map(col => wrapCsvValue(
          typeof col.field === 'function'
            ? col.field(row)
            : row[col.field === void 0 ? col.name : col.field],
          col.format
        )).join(','))
      ).join('\r\n')

      const status = exportFile(
        `${table}.csv`,
        content,
        'text/csv'
      )

      if (status !== true) {
        $q.notify({
          message: 'Browser denied file download...',
          color: 'negative',
          icon: 'warning'
        })
      }
    }

    const tabChanged = () => {
      if (tabName.value === 'rsvp') {
        getData()
      } else {
        getSongs()
      }
      keyword.value = ''
    }

    return {
      filterBy,
      rows: filteredReplies,
      prows: filteredSongs,
      columns,
      pcolumns,
      isLoading,
      tabName,
      replyFilter,
      getData,
      getSongs,
      notGoing,
      going,
      keyword,
      exportTable,
      tabChanged
    }
  }
})
</script>
