<template>
    <div class="table">
        <div class="theader">
            <!-- Pass header data to TableRowComponent -->
            <div class="row">
                <div class="column">

                </div>
            </div>
        </div>
        <div class="tbody">
            <!-- Pass body data to TableRowComponent -->
            <div class="row">
                <div class="column">
                    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TableDataComponent',
    props: {
        data: Array
    }
}
</script>