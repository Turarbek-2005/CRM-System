<script setup lang="ts">
import { useQuery } from "@tanstack/vue-query";
import { COLLECTION_CUSTOMERS, DB_ID } from "~/app.constants";
import type { ICustomer } from "~/types/deals.types";

useSeoMeta({
  title: "Curstomers | CRM System",
});

const { data: customers, isLoading } = useQuery({
  queryKey: ["customers"],
  queryFn: () => DB.listDocuments(DB_ID, COLLECTION_CUSTOMERS),
});
</script>

<template>
  <div class="p-10">
    <h1 class="font-bold text-2xl mb-10">Наши клиенты</h1>
    <div v-if="isLoading">Loading...</div>
    <ShadcnTable v-else>
      <ShadcnTableHeader>
        <ShadcnTableRow>
          <ShadcnTableHead class="w-[80px]">Изображение</ShadcnTableHead>
          <ShadcnTableHead class="w-[200px]">Наименование</ShadcnTableHead>
          <ShadcnTableHead class="w-[200px]">Email</ShadcnTableHead>
          <ShadcnTableHead>Откуда пришел</ShadcnTableHead>
        </ShadcnTableRow>
      </ShadcnTableHeader>
      <ShadcnTableBody>
        <ShadcnTableRow
          v-for="customer in (customers?.documents as unknown as ICustomer[])"
        >
          <ShadcnTableCell>
            <NuxtLink :href="`/customers/edit/${customer.$id}`">
              <NuxtImg
                :src="customer.avatar_url"
                :alt="customer.name"
                width="50"
                height="50"
                class="rounded-full"
              />
            </NuxtLink>
          </ShadcnTableCell>
          <ShadcnTableCell class="font-medium">
            {{ customer.name }}
          </ShadcnTableCell>
          <ShadcnTableCell>{{ customer.email }}</ShadcnTableCell>
          <ShadcnTableCell>{{ customer.from_source }}</ShadcnTableCell>
        </ShadcnTableRow>
      </ShadcnTableBody>
    </ShadcnTable>
  </div>
</template>
