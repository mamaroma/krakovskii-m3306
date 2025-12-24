<script>
    let days = 5;
    let lessons = 6;
    let start = "08:30";
    let dur = 90;
    let gap = 10;

    function toMinutes(time) {
        const [h, m] = time.split(":").map(Number);
        return h * 60 + m;
    }

    function toTime(min) {
        const h = Math.floor(min / 60);
        const m = min % 60;
        return `${String(h).padStart(2, "0")}:${String(m).padStart(2, "0")}`;
    }

    $: slots = Array.from({ length: lessons }, (_, i) => {
        const from = toMinutes(start) + i * (dur + gap);
        return {
            n: i + 1,
            from: toTime(from),
            to: toTime(from + dur)
        };
    });

    $: week = days === 5
        ? ["Пн", "Вт", "Ср", "Чт", "Пт"]
        : ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб"];
</script>

<h2>Конструктор расписания</h2>

<form class="builder-form">
    <label>
        Учебная неделя
        <select bind:value={days}>
            <option value="5">5 дней</option>
            <option value="6">6 дней</option>
        </select>
    </label>

    <label>
        Макс. занятий
        <input type="number" min="1" max="12" bind:value={lessons} />
    </label>

    <label>
        Начало первой пары
        <input type="time" bind:value={start} />
    </label>

    <label>
        Длительность (мин)
        <input type="number" min="20" max="240" bind:value={dur} />
    </label>

    <label>
        Перерыв (мин)
        <input type="number" min="0" max="120" bind:value={gap} />
    </label>
</form>

{#each week as day}
    <section class="card">
        <h3>{day}</h3>

        <table>
            <thead>
            <tr>
                {#each slots as s}
                    <th>
                        Пара {s.n}<br />
                        <small>{s.from}–{s.to}</small>
                    </th>
                {/each}
            </tr>
            </thead>
            <tbody>
            <tr>
                {#each slots as _}
                    <td contenteditable="true"></td>
                {/each}
            </tr>
            </tbody>
        </table>
    </section>
{/each}

<style>
    .builder-form {
        display: grid;
        gap: 0.75rem;
        max-width: 420px;
        margin-bottom: 2rem;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        margin-bottom: 1.5rem;
    }

    th, td {
        border: 1px solid #ccc;
        padding: 0.5rem;
        text-align: center;
    }

    th {
        background: #f0f4ff;
    }
</style>