<template>
  <div>
    <DataTable :value="rows" dataKey="id" paginator rows="0">
      <Column field="message" header="Сообщения"></Column>
    </DataTable>
  </div>
</template>

<script>
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

export default {
  components: {
    DataTable,
    Column,
  },
  data() {
    return {
      rows: [],
    };
  },
  mounted() {
    // Строка, полученная от API
    const apiResponse = `[13:36:53] Расчетное время: 9 мин[13:36:58] Открыть клапан откачки К1[13:36:58] Включить вакуумный насос[13:36:58] Закрыть клапан К5[13:36:58] Закрыть клапан дистилляции К2[13:36:58] Ожидание: 8 с[13:37:06] Заливка 2.2мл. в испаритель[13:37:06] Заданно 26.50602409638554 шагов[13:37:09] Заливка перекиси завершена[13:37:09] Открыть клапан дистилляции К2[13:42:09] Включить нагрев испарителя[13:42:09] Закрыть клапан дистилляции К2[13:42:09] конечное давление1.0960040758227925 торр[13:42:09] Выпаривание через К2[13:42:09] Выпаривание длилось5 мин[13:42:09] Откачка до 1 торр[13:42:15] Закрыть клапан откачки К1[13:43:09] Открыть клапан откачки К1[13:43:14] Аппаратное смещение 0 денсит. = -0.313683180809021[13:43:14] Закрыть клапан дистилляции К2`;

    // Преобразуем строку в массив
    this.rows = this.parseApiResponse(apiResponse);
  },
  methods: {
    parseApiResponse(response) {
      // Проверяем, есть ли сообщения
      const messages = response.match(/(\[\d{2}:\d{2}:\d{2}\].*?)(?=\[\d{2}:\d{2}:\d{2}\]|$)/g);
      
      // Если messages пустой, возвращаем пустой массив
      if (!messages) return [];
      
      // Преобразуем сообщения в формат, необходимый для таблицы
      return messages.map((message, index) => ({ id: index, message: message.trim() }));
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
