<template>
  <div class="about-page">
    <Head>
      <Title>Nosotros</Title>
    </Head>

    <Header theme="dark" />
    <AboutHero
      :text="aboutData.hero_headline" />
    <AboutServices
      :data="aboutData.services" />
    <AboutMethodology
      :data="aboutData.metholodogy" />
    <AboutPartners
      class="js-dark-bg"
      :data="aboutData.partners" />
    <AboutTeam
      :data="aboutData.team" />
    <AboutAwards
      :data="awardsData" />
    <Updates
      theme="light"
      :data="allNews" />
    <Footer theme="light" />
  </div>
</template>

<script>
import mixins from '@/assets/js/mixins';
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'AboutPage',
  mixins: [mixins],
  async setup() {
    const { API_BASE_URL } = useRuntimeConfig();

    let [{acf}, {acf: {awards}}, allNews] = await Promise.all([
      $fetch(`${API_BASE_URL}/pages/79?acf_format=standard`),
      $fetch(`${API_BASE_URL}/pages/2?acf_format=standard`),
      $fetch(`${API_BASE_URL}/updates?per_page=100&acf_format=standard`)
    ]);

    return {
      aboutData: acf,
      awardsData: awards,
      allNews
    }
  },
  data() {
    return {
      changeBgDark: null,
      changeBgLight: null,
    };
  },
  mounted() {
    setTimeout(() => {
      this.changeBgDark = gsap.timeline({
        scrollTrigger: {
          trigger: '.js-dark-bg',
          start: 'top 60%',
          end: '+=500',
          scrub: true,
        }
      })
      .to('.about-page', {
        backgroundColor: '#00001A',
      });
    }, 1000);

    window.addEventListener('resize', () => {
      this.changeBgDark.scrollTrigger.refresh();
      this.changeBgLight.scrollTrigger.refresh();
    })
  },
  beforeDestroy() {
    this.changeBgDark.kill();
    this.changeBgLight.kill();
  }
}
</script>

<style lang="scss" scoped>
  .about-page {
    background-color: $white;
  }
</style>
