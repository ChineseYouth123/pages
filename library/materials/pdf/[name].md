---
title: 资料
---

<script setup>
import { useData, withBase } from "vitepress";

const { params } = useData();
const fileName = params.value.name + ".pdf";
const title = params.value.name;

const pdfModules = import.meta.glob("/pages/library/materials/*.pdf", { eager: true });
const pdfKey = Object.keys(pdfModules).find((k) => k.endsWith(fileName));
const mod = pdfKey ? pdfModules[pdfKey] : null;
const pdfUrl = mod?.default ? withBase(mod.default) : "";
</script>

<PdfContent :title="title" :pdf-url="pdfUrl" />
