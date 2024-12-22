<template>
  <div id="app">
    <h1>Asset List</h1>
    <table border="1">
      <thead>
        <tr>
          <th>Asset Name</th>
          <th>Department</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(asset, index) in assetList" :key="index">
          <td>{{ asset.name }}</td>
          <td>{{ asset.department }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="downloadCSV">Download CSV</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      assets: ["printer", "scanner", "barcode scanner"],
      departments: ["HR", "IT", "Account"],
    };
  },
  computed: {
    assetList() {
      return this.assets.map((name, index) => ({
        name,
        department: this.departments[index] || "Unassigned",
      }));
    },
  },

  methods: {
  downloadCSV() {
    const assetHeaders = "Asset Name,\n";
    const departmentHeaders = "Department,\n";

    const assetContent = this.assets.map(asset => `${asset}`).join("\n");
    const departmentContent = this.departments.map(department => `${department}`).join("\n");

    const csvContent = [
      assetHeaders,
      assetContent,
      "\n\n", // Separate the two lists
      departmentHeaders,
      departmentContent,
    ].join("");

    const blob = new Blob([csvContent], { type: "text/csv" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = "assets_and_departments.csv";
    link.click();
  },
}


  // methods: {
  // downloadCSV() {
  //   const rows = this.assetList;
  //   const headers = "Asset Name,Department\n";
  //   const csvContent = rows
  //     .map(row => `${row.name},${row.department}`)
  //     .join("\n");
  //   const blob = new Blob([headers + csvContent], { type: "text/csv" });
  //   const link = document.createElement("a");
  //   link.href = URL.createObjectURL(blob);
  //   link.download = "assets.csv";
  //   link.click();
  // },
}


</script>

<style>
#app {
  text-align: center;
}
table {
  width: 50%;
  margin: 20px auto;
  border-collapse: collapse;
}
th, td {
  padding: 8px;
  text-align: center;
}
button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
</style>
