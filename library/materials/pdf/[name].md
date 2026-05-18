---
title: 资料
---

<script setup>
import { useData, withBase } from "vitepress";

const { params } = useData();
const title = params.value.name;
const pdfUrl = withBase(`/pages/library/materials/${params.value.name}.pdf`);
</script>

<PdfContent :title="title" :pdf-url="pdfUrl" />
