<template>

  <KGrid>
    <KGridItem
      v-for="content in contents"
      :key="content.id"
      :layout="{ span: cardColumnSpan, alignment: 'auto' }"
    >
      <ChannelCard
        :isMobile="windowIsSmall"
        :title="content.title || content.name"
        :thumbnail="content.thumbnail"
        :tagline="getTagLine(content)"
        :numCoachContents="content.num_coach_contents"
        :link="genContentLinkBackLinkCurrentPage(content.id, false)"
        :isRemote="isRemote"
      />
    </KGridItem>
    <slot></slot>

  </KGrid>

</template>


<script>

  import responsiveWindowMixin from 'kolibri.coreVue.mixins.responsiveWindowMixin';
  import useContentLink from '../composables/useContentLink';
  import ChannelCard from './ChannelCard';

  export default {
    name: 'ChannelCardGroupGrid',
    components: {
      ChannelCard,
    },
    mixins: [responsiveWindowMixin],
    setup() {
      const { genContentLinkBackLinkCurrentPage } = useContentLink();
      return { genContentLinkBackLinkCurrentPage };
    },
    props: {
      contents: {
        type: Array,
        required: true,
      },
      isRemote: {
        type: Boolean,
        default: false,
      },
    },
    computed: {
      cardColumnSpan() {
        if (this.windowBreakpoint <= 2) return 4;
        if (this.windowBreakpoint <= 3) return 6;
        if (this.windowBreakpoint <= 6) return 4;
        return 3;
      },
    },
    methods: {
      getTagLine(content) {
        return content.tagline || content.description;
      },
    },
  };

</script>


<style lang="scss" scoped></style>
