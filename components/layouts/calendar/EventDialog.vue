<script setup lang="ts">
import { ref, watch, defineProps, defineEmits } from 'vue';

const props = defineProps({ event: Object });
const emit = defineEmits(['update', 'close']);

const event = ref({ ...props.event });

onMounted(() => {
    updateEvent();
});
const updateEvent = () => {
    emit('update', event.value);
};
</script>
<template>
    <div class="dialog">
        <form>
            <VField>
                <VInput
                    v-model="event.title"
                    type="text"
                    placeholder="Title"
                    @input="updateEvent"
                />
            </VField>
            <VFlex columnGap="20px">
                <VField>
                    <VInput
                        v-model="event.start"
                        type="datetime-local"
                        required
                        placeholder="Title"
                        @input="updateEvent"
                    />
                </VField>
                <VField>
                    <VInput
                        v-model="event.end"
                        type="datetime-local"
                        required
                        placeholder="Title"
                        @input="updateEvent"
                    />
                </VField>
            </VFlex>
        </form>
    </div>
</template>
