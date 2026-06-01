---
title: 毛泽东
---

# 毛泽东

毛泽东相关电子图书资料。

<script setup>
import { useData } from "vitepress"
import ContentPreview from "@/components/ContentPreview.vue"

const { theme } = useData()
const allFiles = theme.value.maozedongData || []
</script>

<ContentPreview :data="allFiles" :show-categories="false" :show-tags="false" :show-aside="false" />
