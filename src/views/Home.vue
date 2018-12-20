<template>
  <div class="home">{{scss}}</div>
</template>

<style lang="scss" scoped>
.home {
  padding: 0 2rem;
  white-space: pre-wrap;
  text-align: left;
}
</style>


<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios';
import { parse } from 'scss-parser';
import createQueryWrapper from 'query-ast';

import * as scssToJson from 'scss-to-json';
import scssfmt from 'scssfmt';

@Component
export default class Home extends Vue {
  private scss: string = '';
  private created() {
    this.fetchScssFile();
  }

  private async fetchScssFile() {
    try {
      const parsedVars: string[] = [];

      const scss = await axios.get(
        'https://raw.githubusercontent.com/twbs/bootstrap/v4-dev/scss/_variables.scss',
      );
      const fmt = scssfmt(scss.data);
      console.log(fmt);
      this.scss = scssfmt(scss.data).replace(/\/\/.*\n/g, '');
      // console.log(this.scss);

      // const parsed = parse(`$white: #fff; $pointer: $white;`);
      // console.log('s', parsed);

      // const scssAst = parse(scss.data);
      // const scssAst = parse(`
      //   $white: #fff;
      //   $pointer: $white;
      // `);
      // const $ = createQueryWrapper(scssAst);
      // const vars = $(/declaration/);

      // vars.nodes.forEach((declaration) => {
      //   const k = $(declaration)
      //     .find(/variable/)
      //     .value();

      //   const colorHex = $(declaration)
      //     .find(/color_hex/)
      //     .value();

      //   if (colorHex) {
      //     parsedVars.push(`$${k}: #${colorHex}`);
      //   }

      //   console.log(declaration.children);
      // });

      // console.log(parsedVars);
    } catch (err) {
      console.log(err);
    }
  }
}
</script>
