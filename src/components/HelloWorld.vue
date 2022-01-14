<template>
  <v-card-actions>
    <v-tooltip left>
      <template v-slot:activator="{ props }">
        <v-btn v-bind="props">
          <v-icon size="25">mdi-home</v-icon>
        </v-btn>
      </template>
      <span>{{ $t('foo') }}</span>
    </v-tooltip>
  </v-card-actions>
</template>

<i18n src="./msg.json"></i18n>

