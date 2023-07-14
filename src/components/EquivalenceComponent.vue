<template>
  <q-select
    outlined
    v-model="subjectsCompleted"
    :options="OldCurriculum"
    label="Disciplinas concluidas"
    :display-value="subjectsCompleted ? `Total: ${subjectsCompleted.length}` : undefined"
    multiple
    emit-value
    map-options
    options-dense
  >
    <template v-slot:option="{ itemProps, opt, selected, toggleOption }">
      <q-item dense v-bind="itemProps">
        <q-item-section>
          <q-item-label>{{ opt.label }}</q-item-label>
        </q-item-section>
        <q-item-section
          side
          class="q-pa-none"
        >
          <q-checkbox
            :model-value="selected"
            @update:model-value="toggleOption(opt)"
          />
        </q-item-section>
      </q-item>
    </template>
  </q-select>

  <label>Minhas Disciplinas</label>
  <p>Completas: {{  subjectsCompletedNames  }}</p>
  <p>Perdidas: {{ lostSubjects }}</p>
  <p>Equivalentes: {{ oldEquivalents }}</p>
  <p>NewEquivalent: {{ newEquivalent }}</p>
  <p>Aproveitadas: {{ reuse }}</p>

  
  <q-expansion-item
    class="shadow-1 overflow-hidden q-mb-md"
    style="border-radius: 30px"
    icon="task_alt"
    :label="`Disciplinas Aproveitadas ${reuse?.length ?? ''}`"
    header-class="bg-teal-5 text-white text-body1"
    expand-icon-class="text-white"
    >
    <q-list
      class="sbjects__reuse"
      bordered
      separator
    >
    <template v-if="reuse">
      <q-item
        v-for="reuses in reuse"
        :key="reuses"
        class="bg-teal-2 items-center"
        v-ripple
      >{{ reuses }}</q-item>
    </template>
    <template v-else><q-banner>Sem Disciplinas para aproveitar</q-banner></template>
    </q-list>
  </q-expansion-item>

  <q-expansion-item
    class="shadow-1 overflow-hidden q-mb-md"
    style="border-radius: 30px"
    icon="task_alt"
    :label="`Disciplinas Equivalentes ${newEquivalent?.length > 0 ? newEquivalent.length : ''}`"
    header-class="bg-amber-7 text-white text-body1"
    expand-icon-class="text-white"
    >
    <q-list
      class="sbjects__reuse"
      bordered
      separator
    >
    <template v-if="newEquivalent.length > 0">
      <q-list
        class="sbjects__equivalent"
        bordered
        separator
      >
      <q-item
        v-for="equivalent in newEquivalent"
        :key="equivalent"
        class="bg-amber-2 items-center"
        v-ripple
      >{{ equivalent }}</q-item>
    </q-list>
    </template>
    <template v-else><q-banner>Sem Disciplinas Equivalentes</q-banner></template>
    </q-list>
    
  </q-expansion-item>

  <q-expansion-item
    class="shadow-1 overflow-hidden"
    style="border-radius: 30px"
    icon="task_alt"
    :label="`Disciplinas Eliminadas ${lostSubjects?.length ?? ''}`"
    header-class="bg-red-4 text-white text-body1"
    expand-icon-class="text-white"
  >
    <template v-if="lostSubjects">
      <q-list
      class="sbjects__lost"
      bordered
      separator
    >
      <q-item
        v-for="lost in lostSubjects"
        :key="lost"
        class="bg-red-2 items-center"
        v-ripple
      >{{ lost }}</q-item>
    </q-list>
    </template>
    <template v-else><q-banner>Sem Disciplinas perdidas</q-banner></template>
    
  </q-expansion-item>


</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

export interface ICurriculum {
  label: string,
  equivalent: boolean | number,
  reuse: boolean | number,
  unique: boolean,
}

const subjectsCompleted = ref<Array<ICurriculum>>()
const OldCurriculum: ICurriculum[] = ([
  {
    label: 'Coaching e Planejamento de Carreira',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Comunicação Empresarial',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Arquitetura de Computadores do Século XXI',
    equivalent: false,
    reuse: 3,
    unique: false,
  },
  {
    label: 'Criação de Aplicações e Sistemas',
    equivalent: 1,
    reuse: false,
    unique: false,
  },
  {
    label: 'Análise e Levantamento de Requisitos de Software',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Programação Extrema',
    equivalent: 2,
    reuse: false,
    unique: false,
  },
  {
    label: 'Design de Software',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Empreendedorismo e Criação de Novos Negócios',
    equivalent: false,
    reuse: 1,
    unique: false,
  },
  {
    label: 'Desenvolvimento de Aplicações de Banco de Dados',
    equivalent: 6,
    reuse: false,
    unique: false,
  },
  {
    label: 'Prática Integradora Desenvolvimento de Software',
    equivalent: 7,
    reuse: false,
    unique: false,
  }
])

const newCurriculum: ICurriculum[] = ([
  {
    label: 'Html, CSS e Bootstrap',
    equivalent: 7,
    reuse: false,
    unique: false,
  },
  {
    label: 'JavaScript',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'UX, UI & Design Thinking',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Empreendedorismo e Criação de Novos negócios',
    equivalent: false,
    reuse: 1,
    unique: false,
  },
  {
    label: 'Lógica de Programação',
    equivalent: 1,
    reuse: false,
    unique: false,
  },
  {
    label: 'Orientação a Objetos',
    equivalent: false,
    reuse: false,
    unique: true,
  },
  {
    label: 'Java',
    equivalent: 2,
    reuse: false,
    unique: false,
  },
  {
    label: 'Estruturas de Dados para Sistemas Inteligentes',
    equivalent: false,
    reuse: 2,
    unique: false
  }
])

const subjectsCompletedNames = computed(() => subjectsCompleted.value?.map(({label}) => label))

const lostSubjects = computed(() => subjectsCompleted.value?.filter(({unique}) => unique).map(({ label }) => label))

const oldEquivalents = computed(() => subjectsCompleted.value?.filter(({equivalent}) => equivalent).map(({label}) => label)) 

const newEquivalent = computed(() => newCurriculum?.filter(newItem => subjectsCompleted.value?.some(oldItem => (oldItem.equivalent && oldItem.equivalent === newItem.equivalent))).map(newItem => newItem.label))

const reuse = computed(() => subjectsCompleted.value?.filter(subject => subject.reuse).map(({ label }) => label))

</script>

<style scoped lang="scss">
.title { 
  // color: aqua;
}

.title2 {
  color: $green-300;
}
</style>