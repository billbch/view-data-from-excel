<template>
    <h2>list</h2>
    <input type="file" id="archivoExcel" @change="subirExcel()">

    <br>
    <div class="container">
        <input type="text" class="form-control mt-4" placeholder="Search" aria-label="Search" style="width:30%"
            v-model="search_text" v-on:keyup="Search()">
        <div class="table table-responsive table-bordered table-striped">
            <table class="table table-striped mt-3" style="width:100%">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">DOI</th>
                        <th scope="col">Index</th>
                        <th scope="col">Title</th>
                        <th scope="col">Authors</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in items">
                        <th scope="row">1</th>
                        <td>{{ item[0] }}</td>
                        <td>{{ item[1] }}</td>
                        <td>{{ item[2] }}</td>
                        <td>{{ item[3] }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li class="page-item"><a class="page-link" href="#">Previous</a></li>
            <li v-for="item in items" class="page-item">
                <a class="page-link" href="#"><!-- {{ item[0] }} --></a>
            </li>
            <li class="page-item"><a class="page-link" href="#">Next</a></li>
        </ul>
    </nav>

</template>


<script>
import readXlsxFile from 'read-excel-file'

export default {

    data: () => ({
        items: [],
        items_: [],
        perPage: 3,
        currentPage: 1,
        search_text: "",
    }),

    methods: {
        subirExcel() {
            const input = document.getElementById("archivoExcel")
            readXlsxFile(input.files[0]).then((rows) => {
                this.items = rows;
                this.items.shift()
                console.log("asd",this.items)
                this.items_ = this.items;
            })

        },
        Search() {
            this.items = []

            if (this.items_.length != 0) {
                for (var i = 0; i < this.items_.length; i++) {
                    if (String(this.items_[i][3]).toLowerCase().includes(this.search_text.toLowerCase())) {
                        this.items.push(this.items_[i])
                    }
                }
            }
            if (this.search_text.length==0){
                this.items=this.items_
            }
        },

    }
}
</script>

<style>

</style>