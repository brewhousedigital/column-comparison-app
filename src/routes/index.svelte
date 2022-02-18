<script>
    import MisMatchedRow from "$lib/components/MisMatchedRow.svelte";

    let isTableMode = false;

    let isSameNumberOfRows = true;

    //let column0 = "JJuGeO\nobNVZJ\nYCluBI\nxIEmFS\nfQbqgX\nJJuG23232eO\nobN23323211J\nYssssCuBI\nxIEmFS\nfQbqgX";
    let column0 = "";
    let column0RowCount = 0;
    let column0Data = [];

    function handleInput0() {
        let array = column0.split("\n")
        column0RowCount = array.length;
        column0Data = [...array];
    }




    //let column1 = "Sydney Voss\nBad Name1\nZach Voss\nBad Name2\nYuri Voss\nSydney Voss\nBad Name1\nZach Voss\nBad Name2\nYuri Voss";
    let column1 = "";
    let column1RowCount = 0;
    let column1Data = [];

    function handleInput1() {
        let array = column1.split("\n")
        column1RowCount = array.length;
        column1Data = [...array];
    }




    //let column2 = "VOSS\nBAD\nVoss\nLOL\nvoss\nVOSS\nBAD22222\nVoss\nLOL\nvoss";
    let column2 = "";
    let column2RowCount = 0;
    let column2Data = [];

    function handleInput2() {
        let array = column2.split("\n")
        column2RowCount = array.length;
        column2Data = [...array];
    }





    let output = "";
    let outputRowCount = 0;
    let outputData = [];

    function handleCompare() {
        let list = column1Data.map((item, index) => {
            let comparisonList = [item, column2Data[index]]

            if(column0RowCount > 0) {
                comparisonList = [...comparisonList, column0Data[index]]
            }

            return comparisonList;
        })

        let notMatched = list.map((item, index) => {
            let name1 = item[0].toLowerCase();
            let name2 = item[1].toLowerCase();
            let optionalId = item[2];

            if(!name1.includes(name2)) {
                return [index + 1, name1, name2, optionalId]
            } else if(name1 === "" && name2 === "") {
                return [index + 1, "__EMPTY__", "", optionalId]
            } else if(name1 === "") {
                return [index + 1, "__EMPTY__", name2, optionalId]
            } else if(name2 === "") {
                return [index + 1, name1, "__EMPTY__", optionalId]
            } else {
                return null
            }
        })

        notMatched = notMatched.filter(Boolean)

        outputData = [...notMatched];
        outputRowCount = outputData.length;
    }


    $: {
        isSameNumberOfRows = column1RowCount === column2RowCount;
    }
</script>


<div class="container">
    <div class="d-flex align-items-center justify-content-between">
        <h1 class="mb-4">Column Comparison Tool</h1>

        <div class="form-check form-switch">
            <input class="form-check-input"
                   type="checkbox"
                   role="switch"
                   id="table-view-toggle"
                   on:change={() => {isTableMode = !isTableMode}}
                   checked={isTableMode}>
            <label class="form-check-label"
                   for="table-view-toggle">Table View</label>
        </div>
    </div>



    {#if isTableMode}
        <table class="table table-striped table-hover">
            <thead>
            <tr>
                <th scope="col">Row Number</th>
                <th scope="col">ID</th>
                <th scope="col">Column 1 Value</th>
                <th scope="col">Column 2 Value</th>
            </tr>
            </thead>
            <tbody>
            {#each outputData as item}
                <tr>
                    <th scope="row">{item[0]}</th>
                    <td>{item[3]}</td>
                    <td>{item[1]}</td>
                    <td>{item[2]}</td>
                </tr>
            {/each}
            </tbody>
        </table>
    {:else}
        <div class="alert alert-warning" role="alert" style="opacity: {isSameNumberOfRows ? 0 : 1 }">
            Make sure both columns have the same number of rows
        </div>

        <div class="row">
            <div class="col-auto h-100" style="width: 200px">
                <p class="fw-bold">Unique ID</p>
                <p>{column0RowCount} rows total</p>
                <div class="d-flex">
                    <ol style="margin-top: 7px">{#each Array(column0RowCount) as item}<li></li>{/each}</ol>
                    <textarea class="form-control"
                              rows={column0RowCount}
                              bind:value={column0}
                              on:keyup={handleInput0}></textarea>
                </div>
            </div><!-- end col -->

            <div class="col h-100">
                <p class="fw-bold">Column 1 Data</p>
                <p>{column1RowCount} rows total</p>
                <div class="d-flex">
                    <ol style="margin-top: 7px">{#each Array(column1RowCount) as item}<li></li>{/each}</ol>
                    <textarea class="form-control"
                              rows={column1RowCount}
                              bind:value={column1}
                              on:keyup={handleInput1}></textarea>
                </div>
            </div><!-- end col -->

            <div class="col h-100">
                <p class="fw-bold">Column 2 Data</p>
                <p>{column2RowCount} rows total</p>
                <div class="d-flex">
                    <ol style="margin-top: 7px">{#each Array(column2RowCount) as item}<li></li>{/each}</ol>
                    <textarea class="form-control"
                              rows={column2RowCount}
                              bind:value={column2}
                              on:keyup={handleInput2}></textarea>
                </div>
            </div><!-- end col -->


            <div class="col-auto h-100">
                <p class="fw-bold">Output</p>
                <p><button class="btn btn-primary" on:click={handleCompare} disabled={!isSameNumberOfRows}>Compare</button></p>
                <p>{outputRowCount} rows different</p>
                <div>
                    {#each outputData as item}
                        <MisMatchedRow row={item} />
                    {/each}
                </div>
            </div><!-- end col -->
        </div><!-- end row -->
    {/if}

</div><!-- end container -->


<style>

</style>