<template>
  <div class="container mt-4">

    <nav class="nav px-4 py-2 bg-primary rounded">
      <a
        class="navbar-brand rounded-circle"
        href="#"
      >
        <img
          src="./assets/2KcWaF8a_400x400.jpg"
          width="30"
          height="30"
          class="d-inline-block align-top"
          alt=""
          loading="lazy"
        />
      </a>
      <a
        class="nav-link text-light active"
        aria-current="page"
        href="#"
      >素材需求計算機</a>
      <a
        class="nav-link text-light"
        v-b-modal.modal-link
      >其它工具 (外部連結)</a>
    </nav>
    <b-modal
      id="modal-link"
      title="其它工具 (外部連結)"
    >
      <div class="modal-body">
        <ul class="list-group list-group-flush">
          <li class="list-group-item">
            <a
              href="https://arkonegraph.herokuapp.com/"
              target="_blank"
              rel="noopener"
            >明日方舟刷素材推荐一图流</a>
          </li>
          <li class="list-group-item">
            <a
              href="https://arkn.lolicon.app/#/material"
              target="_blank"
              rel="noopener"
            >明日方舟工具箱</a>
          </li>
          <li class="list-group-item">
            <a
              href="https://www.bigfun.cn/tools/aktools-old/akevolve.html"
              target="_blank"
              rel="noopener"
            >明日方舟精英化材料計算器</a>
          </li>
          <li class="list-group-item">
            <a
              href="https://forum.gamer.com.tw/C.php?bsn=33651&snA=628"
              target="_blank"
              rel="noopener"
            >4 星以上幹員培養素材統計</a>
          </li>
          <li class="list-group-item">
            <a
              href="https://gamewith.jp/arknights/article/show/184233"
              target="_blank"
              rel="noopener"
            >公開求人ツール | タグ検索シミュレーター（日文）</a>
          </li>
          <li class="list-group-item">
            <a
              href="https://arknights.wikiru.jp/index.php?%B9%D4%C6%B0%CD%A5%C0%E8%C5%D9%C9%BD"
              target="_blank"
              rel="noopener"
            >行動優先度表（日文）</a>
          </li>
        </ul>
      </div>
    </b-modal>

    <div
      class="mt-2 p-4 bg-light rounded"
      id="app"
    >

      <h1 class="d-inline mr-2">明日方舟素材需求計算機</h1>
      <span class="d-inline-block p-1 bg-dark border rounded text-white lh-1">Last Updated: 2020/07/28</span>

      <h2 class="col-9 mt-4 p-2 border rounded bg-dark text-white">選項</h2>
      <p>選取清單預設為依照稀有度排列，計算結果預設為依照 3 級排列。</p>
      <div class="d-flex flex-wrap">
        <div class="d-flex mr-2 justify-content-center align-items-center">
          <h3 class="d-inline m-0 p-2 border rounded bg-secondary text-white">
            輸入清單排序
          </h3>
          <div class="btn-group ml-2">
            <label
              class="btn btn-outline-primary"
              for="rare"
            ><input
                type="radio"
                class="btn-check"
                name="sort-by-rare"
                id="rare"
                autocomplete="off"
                :value="true"
                v-model="sortByRare"
              />
              稀有度
            </label>

            <label
              class="btn btn-outline-primary"
              for="category"
            ><input
                type="radio"
                class="btn-check"
                name="sort-by-category"
                id="category"
                autocomplete="off"
                :value="false"
                v-model="sortByRare"
              />類型</label>
          </div>
        </div>
        <div class="d-flex mr-2 justify-content-center align-items-center">
          <h3 class="d-inline m-0 p-2 border rounded bg-secondary text-white">
            計算結果稀有度基準
          </h3>
          <div class="btn-group ml-2">
            <label
              class="btn btn-outline-primary"
              for="4"
            ><input
                type="radio"
                class="btn-check"
                name="base-rare"
                id="4"
                autocomplete="off"
                value="4"
                v-model.number="resultBaseRare"
              />4</label>

            <label
              class="btn btn-outline-primary"
              for="3"
            ><input
                type="radio"
                class="btn-check"
                name="base-rare"
                id="3"
                autocomplete="off"
                value="3"
                v-model.number="resultBaseRare"
                checked
              />3</label>

            <label
              class="btn btn-outline-primary"
              for="2"
            ><input
                type="radio"
                class="btn-check"
                name="base-rare"
                id="2"
                autocomplete="off"
                value="2"
                v-model.number="resultBaseRare"
              />2</label>

            <label
              class="btn btn-outline-primary"
              for="1"
            ><input
                type="radio"
                class="btn-check"
                name="base-rare"
                id="1"
                autocomplete="off"
                value="1"
                v-model.number="resultBaseRare"
              />1</label>
          </div>
        </div>
      </div>
      <div class="mt-2">
        <button
          type="button"
          class="btn btn-warning mr-2"
          @click="resetOwn"
        >
          重設擁有
        </button>
        <button
          type="button"
          class="btn btn-warning mr-2"
          @click="resetNeed"
        >
          重設需求
        </button>
        <button
          type="button"
          class="btn btn-danger"
          @click="resetAll"
        >
          重設全部
        </button>
      </div>

      <h2 class="col-9 mt-4 p-2 border rounded bg-dark text-white">輸入</h2>
      <div
        v-if="loading"
        class="spinner-border d-flex justify-content-center"
        role="status"
      >
        <span class="sr-only">Loading...</span>
      </div>
      <div
        v-else
        class="d-flex flex-column justify-content-center"
      >
        <div
          v-for="entries in getMaterialsSorted"
          :key="entries[0].id"
        >
          <h3 class="col-2 p-2 mt-2 border rounded bg-secondary text-white text-center">
            <span v-if="sortByRare">{{ entries[0].rare }}</span>
            <span v-else>{{ entries[0].category }}</span>
          </h3>
          <div class="d-flex flex-wrap">
            <div
              v-for="entry in entries"
              :key="entry.id"
              class="col-md-6 col-lg-4 col-xl-3 border rounded"
            >
              <MaterialCard
                :imgName="entry.img"
                :cn="entry.cn"
                :jp="entry.jp"
                :own="entry.own"
                @input-own="entry.own = $event"
                :need="entry.need"
                @input-need="entry.need = $event"
                @change="getMaterialsNeeded"
              ></MaterialCard>
            </div>
          </div>
        </div>
      </div>

      <h2 class="mt-4 p-2 border rounded bg-dark text-white">結果</h2>
      <div class="d-flex flex-column justify-content-center">
        <div v-if="materialsNeed.length">
          <div
            v-for="entries in materialsNeed"
            :key="entries[0].id"
          >
            <h3 class="col-2 p-2 mt-2 border rounded bg-secondary text-white text-center">
              {{ entries[0].rare }}
            </h3>
            <div class="d-flex flex-wrap">
              <div
                v-for="entry in entries"
                :key="entry.id"
                class="col-md-6 col-lg-4 col-xl-3 border rounded"
              >
                <MaterialCard
                  :imgName="entry.img"
                  :cn="entry.cn"
                  :jp="entry.jp"
                  :own="entry.own"
                  :need="entry.calculatedNeed"
                ></MaterialCard>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>

  </div>
</template>

<script>
  import MaterialCard from "./components/MaterialCard.vue";

  export default {
    components: {
      MaterialCard
    },
    data() {
      return {
        loading: true,
        sortByRare: true,
        maxRare: 5,
        resultBaseRare: 3,
        materials: [
          // {
          //   id: String,
          //   rare: Number,
          //   category: String,
          //   jp: String,
          //   cn: String,
          //   img: String,
          //   recipe: Object,
          //   drop: String,
          //   own: Number,
          //   need: Number
          // }
        ],
        materialsNeed: [],
        categories: {
          a: "エステル素材",
          b: "アケトン素材",
          c: "源岩素材",
          d: "異鉄素材",
          e: "装置素材",
          f: "糖原素材",
          g: "RMA素材",
          h: "合成コール素材",
          i: "砥石素材",
          j: "マンガン素材",
          k: "ゲル素材",
          l: "熾合金素材",
          m: "複合"
        }
      };
    },
    methods: {
      resetOwn: function() {
        for (let i = 0; i < this.materials.length; i++) this.materials.own = 0;
      },
      resetNeed: function() {
        for (let i = 0; i < this.materials.length; i++) this.materials.need = 0;
      },
      resetAll: function() {
        this.resetOwn();
        this.resetNeed();
      },
      getMaterialById: function(id, materials) {
        if (!materials.length) materials = this.materials;
        return materials.find(item => item.id === id);
      },
      getMaterialsSortedByRare: function(materials) {
        const materialsSorted = [];
        for (let i = 1; i <= this.maxRare; i++) {
          const materialPerRare = materials.filter(item => item.rare === i);
          if (materialPerRare.length) materialsSorted.push(materialPerRare);
        }
        // console.log(materialsSorted);
        return materialsSorted;
      },
      getMaterialsSortedByCategory: function(materials) {
        const materialsSorted = [];
        const firstCategory = Object.keys(this.categories)[0],
          categoryLength = Object.keys(this.categories).length - 1,
          lastCategory = Object.keys(this.categories)[categoryLength];
        for (
          let i = firstCategory.charCodeAt(0);
          i <= lastCategory.charCodeAt(0);
          i++
        ) {
          materialsSorted.push(
            materials.filter(
              material => material.category === String.fromCharCode(i)
            )
          );
        }
        // console.log(materialsSorted);
        return materialsSorted;
      },

      convertRecipeToObject: function(recipe) {
        // "X" -> "X"
        // "1e2+2e3+3e4" -> ["1e2", "2e3", "3e4"] -> { e2: 1, e3: 2, e4: 3 }
        const convertResult = {};
        if (recipe === "X") {
          return "X";
        } else if (recipe.indexOf("+")) {
          recipe = recipe.split("+");
        } else {
          recipe = [recipe];
        }
        recipe.forEach(recipeItem => {
          const key = recipeItem.substr(1);
          const value = recipeItem.substr(0, 1);
          convertResult[key] = value;
        });
        return convertResult;
      },
      recipeToBaseRare: function(recipeObject) {
        // key      : "e4"
        // value    : 2
        // baseRare : 3
        // { e4: 2 } -> { e3: 2, c3: 1, i1: 1 }
        if (recipeObject === "X") return "X";
        let allRecipeToBaseRare = false;

        while (!allRecipeToBaseRare) {
          allRecipeToBaseRare = true;
          Object.keys(recipeObject).forEach(recipeKey => {
            const recipeItemRare = recipeKey.slice(1);
            if (parseInt(recipeItemRare) > this.resultBaseRare) {
              allRecipeToBaseRare = false;
              const baseRecipe = this.convertRecipeToObject(
                this.getMaterialById(recipeKey).recipe
              );
              delete recipeObject[recipeKey];
              Object.keys(baseRecipe).forEach(baseKey => {
                if (baseKey in recipeObject) {
                  recipeObject[baseKey] += parseInt(baseRecipe[baseKey]);
                } else {
                  recipeObject[baseKey] = parseInt(baseRecipe[baseKey]);
                }
              });
            }
          });
        }
        return recipeObject;
      },
      getMaterialsNeeded: function() {
        // 取出所有需求材料
        const materialsNeed = this.materials.filter(item => item.need > 0);

        if (!materialsNeed.length) return [];

        // 降解稀有度較高的素材的合成表
        for (let i = 0; i < materialsNeed.length; i++) {
          materialsNeed[i].recipe = this.recipeToBaseRare(
            materialsNeed[i].recipe
          );
        }

        // 計算實際需求量
        // 先加入稀有度較高的素材
        // 將降解後的低階材料數量加至需求陣列中
        const materialsNeedHighRare = materialsNeed.filter(
          item => item.rare > this.resultBaseRare && item.recipe != "X"
        );
        console.log({ materialsNeedHighRare });
        if (materialsNeedHighRare.length) {
          for (let i = 0; i < materialsNeedHighRare.length; i++) {
            const recipeKeys = Object.keys(materialsNeedHighRare[i].recipe);
            recipeKeys.forEach(key => {
              let recipeMaterialIndex = materialsNeed.findIndex(
                material => material.id === key
              );
              console.log({
                materialsNeed,
                recipeKeys,
                i,
                key,
                recipeMaterialIndex
              });
              // 需求材料裡不存在分解後的低階材料
              if (recipeMaterialIndex === -1) {
                materialsNeed.push(this.getMaterialById(key));
                recipeMaterialIndex = materialsNeed.findIndex(
                  material => material.id === key
                );
              }
              materialsNeed[recipeMaterialIndex].calculatedNeed +=
                parseInt(materialsNeedHighRare[i].recipe[key]) *
                parseInt(materialsNeedHighRare[i].need);
            });
          }
        }

        const materialsNeedLowRare = materialsNeed.filter(
          item => item.rare <= this.resultBaseRare || item.recipe === "X"
        );
        console.log({ materialsNeedLowRare });
        if (materialsNeedLowRare.length) {
          for (let i = 0; i < materialsNeedLowRare.length; i++) {
            let recipeMaterialIndex = materialsNeed.findIndex(
              material => material.id === materialsNeedLowRare[i].id
            );
            materialsNeed[recipeMaterialIndex].calculatedNeed += parseInt(
              materialsNeedLowRare[i].need
            );
          }
        }

        // return this.getMaterialsSortedByRare(materialsNeed);
        this.materialsNeed = this.getMaterialsSortedByRare(materialsNeed);
      }
    },
    computed: {
      getMaterialsSorted: function(materials) {
        if (!materials.length) materials = this.materials;
        if (this.sortByRare) return this.getMaterialsSortedByRare(materials);
        else return this.getMaterialsSortedByCategory(materials);
      }
    },
    mounted() {
      this.$http
        .get(
          "https://spreadsheets.google.com/feeds/cells/1pQHkQCVECL0t7XSgkcveA2WViZ3G73SnSSPGk8HNDXQ/2/public/values?alt=json"
        )
        .then(res => {
          for (let i = 1; i <= res.data.feed.gs$rowCount.$t; i++) {
            const materials_temp = [];
            // get material data from every row
            res.data.feed.entry
              .filter(item => item.gs$cell.row == i)
              .forEach(item => {
                materials_temp.push(item.content.$t);
              });
            // save material datas
            this.materials.push({
              id: materials_temp[0],
              rare: parseInt(materials_temp[1]),
              category: materials_temp[0][0],
              jp: materials_temp[2],
              cn: materials_temp[3],
              img: materials_temp[4],
              recipe: this.convertRecipeToObject(materials_temp[5]),
              drop: materials_temp[6],
              own: 0,
              need: 0,
              calculatedNeed: 0
            });
          }
        })
        .catch(err => {
          console.log(err);
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
          // console.log(this.materials);
        });
    }
  };
</script>

<style lang="scss">
  @import url("https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap");

  body {
    font-family: "Noto Sans JP", sans-serif !important;
  }

  h4 {
    font-size: 1.25rem !important;
  }

  [type="radio"] {
    display: none;
  }
</style>
