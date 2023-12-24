<script>
    import "../app.css";
</script>

<slot />

<style>
  /* 전역 폰트family를 ../fonts/Roboto/Roboto-Regular.ttf 로 설정 */
  :global(html) {
    font-family: "Roboto", sans-serif;
  }
</style>