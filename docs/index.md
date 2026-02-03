---
hide:
  - toc
---

<div id="spcalc-root">Loading calculator…</div>

<script>
(async () => {
  let manifest;
  try {
    const res = await fetch("/assets/calculator/manifest.json", { cache: "no-cache" });
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    manifest = await res.json();
  } catch (err) {
    console.error("Failed to load Vite manifest", err);
    const root = document.getElementById("spcalc-root");
    if (root) root.textContent = "Failed to load calculator assets.";
    return;
  }

  const entry = manifest["index.html"];
  if (!entry) {
    console.error("Vite manifest missing index.html entry", manifest);
    return;
  }

  (entry.css || []).forEach((href) => {
    const link = document.createElement("link");
    link.rel = "stylesheet";
    link.href = "/assets/calculator/" + href.replace(/^assets\//, "");
    document.head.appendChild(link);
  });

  const script = document.createElement("script");
  script.type = "module";
  script.src = "/assets/calculator/" + entry.file.replace(/^assets\//, "");
  document.head.appendChild(script);
  const root = document.getElementById("spcalc-root");
  if (root) root.textContent = "";
})();
</script>
