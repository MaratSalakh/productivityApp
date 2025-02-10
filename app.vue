<script setup lang="ts">
import { navigationMenuTriggerStyle } from "~/components/ui/navigation-menu";
</script>

<template>
  <NavigationMenu class="m-4">
    <NavigationMenuList>
      <NavigationMenuItem :class="navigationMenuTriggerStyle()"
        ><NavigationMenuLink href="/" :class="navigationMenuTriggerStyle()"
          >Productivity App</NavigationMenuLink
        ></NavigationMenuItem
      >
      <NavigationMenuItem
        ><NavigationMenuLink
          href="/todolist"
          :class="navigationMenuTriggerStyle()"
        >
          Todolist
        </NavigationMenuLink>
      </NavigationMenuItem>
    </NavigationMenuList>
  </NavigationMenu>
  <NuxtPage></NuxtPage>
</template>
