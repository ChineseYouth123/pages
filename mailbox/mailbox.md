---
title: 信箱
aside: false
---

<script setup>
import { useData } from "vitepress"
import ContentPreview from "@/components/ContentPreview.vue"

const { theme } = useData()
const allFiles = theme.value.mailboxData || []
</script>

<ContentPreview :data="allFiles" :show-categories="true" :show-tags="true" :show-aside="true" />
