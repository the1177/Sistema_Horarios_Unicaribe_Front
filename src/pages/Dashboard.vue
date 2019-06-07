<template>
  <div id="pageDashboard">
    <v-container grid-list-xl fluid>
      <v-layout row wrap>    
        <v-flex lg6>
          <v-widget title="Bienvenido">
            <div slot="widget-content">
              <v-jumbotron color="grey lighten-2">
                <v-container fill-height>
                  <v-layout align-center>
                    <v-flex>
                      <h3 class="display-3">Bienvenido</h3>
                      <span class="subheading">Lorem ipsum dolor sit amet, pri veniam forensibus id. Vis maluisset molestiae id, ad semper lobortis cum. At impetus detraxit incorrupte usu, repudiare assueverit ex eum, ne nam essent vocent admodum.</span>
                      <v-divider class="my-3"></v-divider>
                      <div class="title mb-3">Lorem ipsum dolor sit ame</div>
                      <v-btn large color="primary" class="mx-0">Ver más</v-btn>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-jumbotron>
            </div>
          </v-widget>
        </v-flex>           
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import API from '@/api';
import EChart from '@/components/chart/echart';
import MiniStatistic from '@/components/widgets/statistic/MiniStatistic';
import PostListCard from '@/components/widgets/card/PostListCard';
import ProfileCard from '@/components/widgets/card/ProfileCard';
import PostSingleCard from '@/components/widgets/card/PostSingleCard';
import WeatherCard from '@/components/widgets/card/WeatherCard';
import PlainTable from '@/components/widgets/list/PlainTable';
import PlainTableOrder from '@/components/widgets/list/PlainTableOrder';
import VWidget from '@/components/VWidget';
import Material from 'vuetify/es5/util/colors';
import VCircle from '@/components/circle/VCircle';
import BoxChart from '@/components/widgets/chart/BoxChart';
import ChatWindow from '@/components/chat/ChatWindow';
import CircleStatistic from '@/components/widgets/statistic/CircleStatistic';
import LinearStatistic from '@/components/widgets/statistic/LinearStatistic';

export default {
  components: {
    VWidget,
  },
  data () {
    return {
      gradient: 'to top, #7B1FA2, #E1BEE7',
      gradient2: 'to top right, rgba(63,81,181, .7), rgba(25,32,72, .7)'
    };
  },

};
</script>
