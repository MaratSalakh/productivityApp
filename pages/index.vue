<script lang="ts" setup>
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardFooter } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import duration from "dayjs/plugin/duration";
import dayjs from "dayjs";

dayjs.extend(duration);

const tasks = ref<
  {
    name: string;
    description: string;
    id: number;
    time: number;
    intervalID: null | NodeJS.Timeout;
    changing: boolean;
  }[]
>([]);

let ID = 0;

const addTask = () => {
  tasks.value.push({
    name: "check",
    description: "check descrip",
    id: ID,
    time: 0,
    intervalID: null,
    changing: true,
  });

  const taskInArray = tasks.value.filter((task) => task.id === ID);

  ID += 1;

  setTimeout(() => {
    taskInArray[0].changing = false;
  }, 100);
};

const deleteTask = (id: number) => {
  const taskInArray = tasks.value.filter((task) => task.id === id);
  const intervalID = taskInArray[0].intervalID;
  taskInArray[0].changing = true;

  if (intervalID !== null) {
    clearInterval(intervalID);
  }

  const newTasks = tasks.value.filter((task) => task.id !== id);

  setTimeout(() => (tasks.value = newTasks), 300);
};

const startTask = (id: number) => {
  const taskInArray = tasks.value.filter((task) => task.id === id);

  const intervalID = setInterval(() => {
    taskInArray[0].time += 1;

    console.log(id);
  }, 1000);

  taskInArray[0].intervalID = intervalID;
};

const stopTask = (id: number) => {
  const taskInArray = tasks.value.filter((task) => task.id === id);
  const intervalID = taskInArray[0].intervalID;

  if (intervalID !== null) {
    clearInterval(intervalID);
  }

  taskInArray[0].intervalID = null;
};

const taskIsActive = (id: number) => {
  const taskInArray = tasks.value.filter((task) => task.id === id);
  const intervalID = taskInArray[0].intervalID;

  return intervalID;
};

const changingCheck = (changing: boolean) =>
  changing ? "opacity-0" : "opacity-100";

function timeFormatter(seconds: number) {
  // Создаем объект dayjs с заданными секундами
  const duration = dayjs.duration(seconds * 1000); // Умножаем на 1000 для преобразования в миллисекунды

  // Получаем минуты и секунды
  const minutes = duration.minutes();
  const remainingSeconds = duration.seconds();

  // Форматируем результат
  return `${minutes.toString().padStart(2, "0")}:${remainingSeconds
    .toString()
    .padStart(2, "0")}`;
}
</script>

<template>
  <div class="m-8"><Button @click="addTask">Add task</Button></div>
  <div class="m-8 flex flex-row justify-start align-middle flex-wrap">
    <Card
      v-for="task in tasks"
      :key="task.id"
      :class="changingCheck(task.changing)"
      class="w-[320px] pt-6 m-2 transition-opacity duration-100 ease-in"
    >
      <CardContent>
        <form>
          <div class="grid items-center w-full gap-4">
            <div class="flex flex-col space-y-1.5">
              <Label for="name">Task</Label>
              <Input id="name" placeholder="Write smth..." />
            </div>
          </div>
        </form>
      </CardContent>
      <CardFooter class="flex justify-between px-6 pb-6">
        <Button @click="deleteTask(task.id)" variant="outline">Delete</Button>
        <div>{{ timeFormatter(task.time) }}</div>
        <div>
          <Button
            :disabled="!taskIsActive(task.id)"
            :variant="taskIsActive(task.id) ? 'default' : 'outline'"
            class="mr-2"
            @click="stopTask(task.id)"
            >Stop</Button
          >
          <Button
            :disabled="taskIsActive(task.id)"
            :variant="taskIsActive(task.id) ? 'outline' : 'default'"
            @click="startTask(task.id)"
            >Start</Button
          >
        </div>
      </CardFooter>
    </Card>
  </div>
</template>

<style scoped></style>
