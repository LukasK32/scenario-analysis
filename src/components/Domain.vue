<template>
  <div class="card" v-if="domain !== null">
    <div class="card-content">
      <div class="media columns is-vcentered">

        <div class="column is-8">
          <p class="title is-5">
            {{ domain.name }}
          </p>
        </div>

        <div class="column buttons has-text-right">
          <button class="button is-info" @click="edit">Edytuj</button>
          <button class="button is-danger" @click="destroy">Usuń</button>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    ID: {
      default: '0',
    },
  },
  computed: {
    domain() {
      const { domains } = this.$store.state;

      if (this.ID in domains) return domains[this.ID];

      return null;
    },
  },
  methods: {
    edit() {
      this.$buefy.dialog.prompt({
        message: `Podaj nową nazwę dla sfery <strong>${this.domain.name}</strong>:`,
        inputAttrs: {
          placeholder: 'np. Ekonomiczna',
          maxlength: 50,
          value: this.domain.name,
        },
        cancelText: 'Anuluj',
        confirmText: 'Zapisz',
        trapFocus: true,
        onConfirm: this.update,
      });
    },
    update(name) {
      if (name === null || name === '' || name === this.domain.name) return;

      this.$store.commit('updateDomain', {
        ID: this.ID,
        domain: {
          name,
        },
      });
    },
    destroy() {
      this.$buefy.dialog.confirm({
        message: `Czy napewno chcesz usunąć sferę <strong>${this.domain.name}</strong>?`,
        cancelText: 'Anuluj',
        confirmText: 'Usuń',
        type: 'is-danger',
        onConfirm: () => this.$store.dispatch('destroyDomain', this.ID),
      });
    },
  },
};
</script>
