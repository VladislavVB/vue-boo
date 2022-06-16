<template>
  <b-container fluid>
    <!-- User Interface controls -->
    <b-row>
      <b-col lg="10" class="my-1">
        <b-form-group
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''"
                >Clear</b-button
              >
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
      </b-col>
      <!-- perpage -->
      <b-col lg="2" md="6" class="my-1">
        <b-form-group
          label-for="per-page-select"
          label-cols-sm="6"
          label-cols-md="4"
          label-cols-lg="3"
          label-align-sm="right"
          label-size="sm"
          class="mb-0"
        >
          <b-form-select
            id="per-page-select"
            v-model="perPage"
            :options="pageOptions"
            size="sm"
          ></b-form-select>
        </b-form-group>
      </b-col>
    </b-row>

    <!-- Main table element -->
    <div class="castom-scroll">
      <b-table
        :items="items"
        :fields="fields"
        :current-page="currentPage"
        :per-page="perPage"
        :filter="filter"
        :filter-included-fields="filterOn"
        :sort-by.sync="sortBy"
        :sort-desc.sync="sortDesc"
        :sort-direction="sortDirection"
        stacked="md"
        show-empty
        small
        label-sort-asc=""
        label-sort-desc=""
        label-sort-clear=""
        @filtered="onFiltered"
      >
        <template #cell(name)="row">
          {{ row.value.first }} {{ row.value.last }}
        </template>

        <template #cell(actions)="row">
          <b-button
            size="sm"
            @click="info(row.item, row.index, $event.target)"
            class="mr-1"
          >
            Info modal
          </b-button>
          <b-button size="sm" @click="row.toggleDetails">
            {{ row.detailsShowing ? "Hide" : "Show" }} Details
          </b-button>
        </template>

        <template #row-details="row">
          <b-card>
            <ul>
              <li v-for="(value, key) in row.item" :key="key">
                {{ key }}: {{ value }}
              </li>
            </ul>
          </b-card>
        </template>
      </b-table>
    </div>

    <!-- pagination  -->
    <b-col sm="12" md="12" class="my-1">
      <b-pagination
      pills 
        :total-rows="totalRows"
        v-model="currentPage"
        :per-page="perPage"
        align="center"
        size="sm"
        use-router
        class="my-0"
      ></b-pagination>
    </b-col>

    <!-- Info modal -->
    <b-modal
      :id="infoModal.id"
      :title="infoModal.title"
      ok-only
      @hide="resetInfoModal"
    >
      <pre>{{ infoModal.content }}</pre>
    </b-modal>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        }, {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        }, {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        }, {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        }, {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 81",
          email: "ratata@mail.ru",
          postIndex: "302909",
          age: 40,
          name: { first: "Thor", last: "Macdonald" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 81",
          email: "ratata@mail.ru",
          postIndex: "302909",
          age: 40,
          name: { first: "Thor", last: "Macdonald" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 81",
          email: "ratata@mail.ru",
          postIndex: "302909",
          age: 40,
          name: { first: "Thor", last: "Macdonald" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 89,
          name: { first: "Geneva", last: "Wilson" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 38,
          name: { first: "Jami", last: "Carney" },
        },
        {
          isActive: false,
          phone: "8 888 888 88 88",
          email: "ratata@mail.ru",
          postIndex: "302900",
          age: 27,
          name: { first: "Essie", last: "Dunlap" },
        },
        {
          isActive: true,
          phone: "8 888 888 88 81",
          email: "ratata@mail.ru",
          postIndex: "302909",
          age: 40,
          name: { first: "Thor", last: "Macdonald" },
        },
      ],
      fields: [
        {
          key: "name",
          label: "Имя пользователя",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "phone",
          label: "Номер телефона",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "email",
          label: "email",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "postIndex",
          label: "Почтовый индекс",
          sortable: true,
          sortDirection: "desc",
        },
        {
          key: "age",
          label: "Возраст",
          sortable: true,
          class: "text-center",
        },
        {
          key: "isActive",
          label: "Статус",
          formatter: (value, key, item) => {
            return value ? "Yes" : "No";
          },
          sortable: true,
          sortByFormatted: true,
          filterByFormatted: true,
        },
        { key: "actions", label: "Actions" },
      ],
      totalRows: 1,
      currentPage: 1,
      perPage: 5,
      pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
      sortBy: "",
      sortDesc: false,
      sortDirection: "asc",
      filter: null,
      filterOn: [],
      infoModal: {
        id: "info-modal",
        title: "",
        content: "",
      },
    };
  },
  computed: {
    sortOptions() {
      // Create an options list from our fields
      return this.fields
        .filter((f) => f.sortable)
        .map((f) => {
          return { text: f.label, value: f.key };
        });
    },
  },
  mounted() {
    // Set the initial number of items
    this.totalRows = this.items.length;
  },
  methods: {
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`;
      this.infoModal.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal.id, button);
    },
    resetInfoModal() {
      this.infoModal.title = "";
      this.infoModal.content = "";
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length;
      this.currentPage = 1;
    },
  },
};
</script>
<style lang="scss">
.castom {
  &-scroll {
    width: 100%;
    overflow-x: scroll;
    table {
       width: 2000px;
    }
  }
}
</style>