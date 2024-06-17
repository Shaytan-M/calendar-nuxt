<script setup lang="ts">
const pageTitle = useState('page-title');
onMounted(() => {
    pageTitle.value = 'Calendar';
});
</script>

<template>
    <div>
        <EventCalendar />
    </div>
</template>
