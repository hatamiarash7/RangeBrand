<template>
    <Layout>
        <div class="h-screen -mt-20 pt-16">
            <color-banner :colors="colors" @update="updateQuery"/>
        </div>
    </Layout>
</template>
<script>
import ColorBanner from '~/components/colors/banner';

import { validateHex } from '~/scripts/utils/validator';

export default {
    components: {
        ColorBanner,
    },
    computed: {
        colors() {
            try {
                return this.$route.query.colors.split('-').filter((item, index, array) => {
                    return validateHex(item) && array.indexOf(item) === index;
                });
            } catch {
                return [];
            }
        },
    },
    beforeMount() {
        if (!this.colors.length) {
            this.$router.replace({
                path: '/colors/',
            });
        }
    },
    methods: {
        updateQuery(value) {
            const newColors = value.join('-');
            if (newColors !== this.$route.query.colors) {
                this.$router.push({
                    path: '/palette',
                    query: {
                        colors: value.join('-'),
                    },
                });
            }
        },
    },
};
</script>
