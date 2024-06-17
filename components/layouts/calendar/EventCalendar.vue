<script setup lang="ts">
import VCal from 'vue-cal';
import 'vue-cal/dist/vuecal.css';

export interface ECalendarEvent {
    start?: string;
    end?: string;
    title?: string;
    id: string;
}
const events = ref<ECalendarEvent[]>([
    { start: '2024-06-20 10:00', end: '2024-06-20 12:00', title: 'Meeting', id: '1' },
    { start: '2024-06-21 14:00', end: '2024-06-21 16:00', title: 'Conference', id: '2' },
]);

const dialogVisible = ref(false);
const selectedEvent = ref({});
const modalTitle = ref('');

const parseDate = (date: string) => {
    const dateObj = new Date(date);
    const year = dateObj.getFullYear();
    const month = String(dateObj.getMonth() + 1).padStart(2, '0'); // Місяці від 0 до 11
    const day = String(dateObj.getDate()).padStart(2, '0');
    const hours = String(dateObj.getHours()).padStart(2, '0');
    const minutes = String(dateObj.getMinutes()).padStart(2, '0');
    const formattedDate = `${year}-${month}-${day} ${hours}:${minutes}`;
    return formattedDate;
};
const addHour = (datetime: string): string => {
    const dateObj = new Date(datetime);
    dateObj.setHours(dateObj.getHours() + 1);
    return dateObj.toString();
};

const handleCellClick = (cell: string) => {
    const DefaultEnd = parseDate(addHour(cell));
    selectedEvent.value = { start: parseDate(cell), end: DefaultEnd, title: '' };
    modalTitle.value = 'New Event';
    dialogVisible.value = true;
};
const handleEventClick = (event) => {
    const eventForEdit = {
        start: parseDate(event.start),
        end: parseDate(event.end),
        title: event.title,
        id: event.id,
    };
    selectedEvent.value = { ...eventForEdit };
    modalTitle.value = 'Edit Event';
    dialogVisible.value = true;
};

const handleSaveEvent = (event: ECalendarEvent) => {
    event.start = event.start?.replace('T', ' ');
    event.end = event.end?.replace('T', ' ');
    event.id = event.id ?? crypto.randomUUID();
    const existingEventIndex = events.value.findIndex((e: ECalendarEvent) => e.id === event.id);
    if (existingEventIndex !== -1) {
        events.value[existingEventIndex] = event;
    } else {
        events.value.push(event);
    }
    selectedEvent.value = {};
    dialogVisible.value = false;
};
</script>
<template>
    <div>
        <div class="calendar-wrapper">
            <v-cal
                :events="events"
                :time="true"
                @cell-click="handleCellClick"
                @event-click="handleEventClick"
                :disable-views="['years', 'year']"
                :editable-events="{
                    drag: true,
                    delete: true,
                }"
            />
        </div>
        <VModal
            :title="modalTitle"
            :open="dialogVisible"
            size="medium"
            @close="dialogVisible = false"
            @save="handleSaveEvent"
        >
            <template #content="{ setDialogData }">
                <EventDialog :event="selectedEvent" @update="setDialogData" />
            </template>
        </VModal>
    </div>
</template>
