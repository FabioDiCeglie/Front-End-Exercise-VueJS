<script setup>
const layout = {
  ranks: ["rank1", "rank2", "rank3"],
  sections: [
    {
      name: "main hall",
      rows: [
        {
          row: "1",
          seats: [
            { seat: "1", rank: "rank1" },
            { seat: "3", rank: "rank1" },
            { seat: "4", rank: "rank1" },
            { seat: "2", rank: "rank1" },
          ],
        },
        {
          row: "2",
          seats: [
            { seat: "1", rank: "rank1" },
            { seat: "3", rank: "rank1" },
            { seat: "5", rank: "rank1" },
            { seat: "6", rank: "rank1" },
            { seat: "4", rank: "rank1" },
            { seat: "2", rank: "rank1" },
          ],
        },
        {
          row: "3",
          seats: [
            { seat: "1", rank: "rank2" },
            { seat: "3", rank: "rank2" },
            { seat: "5", rank: "rank2" },
            { seat: "6", rank: "rank2" },
            { seat: "4", rank: "rank2" },
            { seat: "2", rank: "rank2" },
          ],
        },
      ],
    },
  ],
};
const groups = [
  {
    id: "+31611111111",
    seats: [
      { section: "main hall", row: "1", seat: "4" },
      { section: "main hall", row: "1", seat: "2" },
      { section: "main hall", row: "2", seat: "2" },
      { section: "main hall", row: "2", seat: "4" },
    ],
  },
  {
    id: "+31622222222",
    seats: [
      { section: "main hall", row: "2", seat: "6" },
      { section: "main hall", row: "2", seat: "5" },
    ],
  },
];

//BATTLEPAN
// We need to map over the seats and check if the section name, the row and the seat in the group exist
// If that exist I add the group id(that identify the group) in the object of the layoutSeat

const checkSection = layout.sections.map((layoutSection) => {
  const checkLayoutSection = layoutSection.rows.map((layoutRow) => {
    const checkLayoutSeat = layoutRow.seats.map((layoutSeat) => {
      let seatTaken = null;

      groups.forEach((group) =>
        group.seats.forEach((groupSeat) => {
          const { section, row, seat } = groupSeat;
          if (
            section == layoutSection.name &&
            row === layoutRow.row &&
            seat === layoutSeat.seat
          ) {
            seatTaken = group.id;
          }
        })
      );
      if (seatTaken) {
        return { ...layoutSeat, seatTaken };
      } else {
        return { ...layoutSeat };
      }
    });

    return { ...layoutRow, seats: checkLayoutSeat };
  });
  return { ...layoutSection, rows: checkLayoutSection };
});
console.log(checkSection);

//HardCode section first try and then I made it dynamic on top!
// const sections = (i, r) => {
//   const seatRow = seat[i];

//   const findRow = layout.sections.map((section) =>
//     section.rows.find((row) => row.row === r)
//   );
//   //console.log(findFirstRow);
//   const modifyRow = findRow.map((row) => {
//     return { ...row, seats: seatRow };
//   });
//   return modifyRow;
// };
// const modifyFirstRow = sections(0, "1");
// const modifySecondRow = sections(1, "2");
// const modifyThirdRow = sections(2, "3");
//console.log(modifyFirstRow);

//Make a condition to check the rank and if the seat was taken
//With this condition I can change the class name and change the css of the app
const rankStatus = (rank) => {
  if (rank === "rank1") {
    return "rank1";
  } else if (rank === "rank2") {
    return "rank2";
  }
};
const seatStatus = (seatTaken) => {
  if (seatTaken === "+31611111111") {
    return "group1";
  } else if (seatTaken === "+31622222222") {
    return "group2";
  }
};
</script>

<template>
  <main>
    <div>
      <div class="header" v-for="{ name, rows } in checkSection">
        <h1 class="headerTitle">{{ name }}</h1>
        <div class="row" v-for="{ row, seats } in rows">
          <h1>Row {{ row }}</h1>
          <div
            v-for="{ seat, rank, seatTaken } in seats"
            v-bind:class="!seatTaken ? rankStatus(rank) : seatStatus(seatTaken)"
          >
            <p>Seat {{ seat }}</p>
            <p>{{ rank }}</p>
            <p>{{ seatTaken }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
@import "./assets/base.css";

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  font-weight: normal;
  background-color: rgb(239, 239, 255);
}

.header {
  text-align: center;
  margin-bottom: 10%;
  background-color: rgb(176, 176, 255);
}

.headerTitle {
  margin-bottom: 5%;
}
.row {
  display: flex;
  margin-bottom: 70px;
  margin-left: 30px;
}

.group1 {
  background-color: aqua;
  margin-right: 60px;
  margin-left: 10px;
  padding: 10px;
  color: black;
  width: 10%;
}
.group2 {
  background-color: red;
  margin-right: 60px;
  margin-left: 10px;
  padding: 10px;
  color: black;
  width: 10%;
}

.rank1 {
  display: flex;
  flex-direction: column;
  padding: 10px;
  background-color: yellow;
  color: black;
  margin-right: 60px;
  margin-left: 10px;
  width: 10%;
}
.rank2 {
  display: flex;
  flex-direction: column;
  padding: 10px;
  background-color: black;
  color: white;
  margin-right: 60px;
  margin-left: 10px;
  width: 10%;
}
</style>
