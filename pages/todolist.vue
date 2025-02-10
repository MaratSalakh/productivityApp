<script setup lang="ts">
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

let ID = 0;

const tasks = ref<{ ended: boolean; id: number }[]>([]);

const createTask = () => {
  tasks.value.push({ ended: false, id: (ID += 1) });
};

const deleteTask = (id: number) => {
  tasks.value = tasks.value.filter((task) => task.id !== id);
};

const changeTaskStatus = (id: number) => {
  const task = tasks.value.filter((task) => task.id === id);

  task[0].ended = !task[0].ended;
};
</script>

<template>
  <div class="m-8"><Button @click="createTask">Add task</Button></div>

  <div class="flex justify-start flex-wrap m-8">
    <Card
      v-for="task in tasks"
      class="w-[320px] m-4"
      :class="task.ended ? 'bg-gray-100' : ''"
    >
      <div class="flex justify-end">
        <Button @click="deleteTask(task.id)" size="xs" variant="secondary">
          X
        </Button>
      </div>
      <CardContent>
        <form>
          <div class="flex justify-between content-center">
            <div class="flex flex-col space-y-1.5 w-full">
              <Label for="task">Task</Label>
              <div class="flex flex-row justify-between items-center space-x-4">
                <Input
                  :disabled="task.ended"
                  id="task"
                  placeholder="Write smth..."
                />
                <Checkbox
                  type="checkbox"
                  :checked="task.ended"
                  @click="changeTaskStatus(task.id)"
                  class="border-gray-300"
                />
              </div>
            </div>
          </div>
        </form>
      </CardContent>
    </Card>
  </div>
</template>
