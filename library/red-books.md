---
title: 红色书籍
---

# 红色书籍

红色经典相关电子图书资料。

<script setup>
import { useData } from "vitepress"
import ContentPreview from "@/components/ContentPreview.vue"

const { theme } = useData()
const allFiles = theme.value.redBooksData || []
</script>

<ContentPreview :data="allFiles" :show-categories="false" :show-tags="false" :show-aside="false" />
