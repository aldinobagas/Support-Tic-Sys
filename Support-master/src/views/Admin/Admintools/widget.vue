<v-template>
	<v-container fluid grid-list-xl>
	<v-flex d-flex lg3 sm6 xs12>
        <widget icon="domain" title="1,287,687" :subTitle="$t('widgetHigherYesterday', ['13%'])" :supTitle="$t('widgetTodaysVisit')" color="#00b297"/>
      </v-flex>
      <v-flex d-flex lg3 sm6 xs12>
        <widget icon="money_off" title="$141,291" :subTitle="$t('widgetBeforeTax', ['$117,212'])" :supTitle="$t('widgetTodaysSale')" color="#dc3545"/>
      </v-flex>
      <v-flex d-flex lg3 sm6 xs12>
        <widget icon="computer" title="33.45%" :subTitle="$t('widgetAverageDuration', ['13%'])"  :supTitle="$t('widgetUniqueVisits')" color="#0866C6"/>
      </v-flex>
      <v-flex d-flex lg3 sm6 xs12>
        <widget icon="watch_later" title="13.00%" :subTitle="$t('widgetAverageTime', ['17.25%'])" :supTitle="$t('widgetBounceRate')" color="#1D2939"/>
      </v-flex>
  </v-container>
</v-template>
<script></script>
<style></style>>