<script>
    import Button from "smelte/src/components/Button";
    let current_page = 1;
    let from = 1;
    let to = 1;
    let per_page = 1;
    let last_page = 1;
    let total = 0;
    let rows = [];

    let loading = true;
</script>

<Button class="pagination back">BACK</Button>
<Button class="pagination forward">FORWARD</Button>
