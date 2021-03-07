<template>
<div class="admin">
  <div v-if="this.$root.$data.rCurrentUser.role == 'admin'">
    <h1>Admin Page</h1>
    <div class="heading">
      <h2>Add a Recipe</h2>
    </div>
    <div class="add">
      <div class="form">
        <input class="title-input" v-model="title" placeholder="Click to add Title">
        <p></p>
        <input type="file" ref="imageUpload" name="photo" @change="fileChanged">
        <p></p>
        <p class="header">Ingredients</p>
        <div class="ingredients-input">
          <div class="ingredient-sublist-input">
            <input class="ingredient-sublist-header-input" v-model="ingredientsHeader1" placeholder=" Add Ingredients Header 1">
            <p></p>
            <textarea class="ingredient-sublist-content-input" v-model="ingredientsList1" placeholder="Add Ingredients List 1"></textarea>
            <p></p>
          </div>
          <div class="ingredient-sublist-input">
            <input class="ingredient-sublist-header-input" v-model="ingredientsHeader2" placeholder="Add Ingredients Header 2">
            <p></p>
            <textarea class="ingredient-sublist-content-input" v-model="ingredientsList2" placeholder="Add Ingredients List 2"></textarea>
            <p></p>
          </div>
        </div>
        <hr>
        <p class="header">Instructions</p>
        <textarea class="instructions-input" v-model="instructionList" placeholder="Add Instruction List"></textarea>
        <p></p>
        <input v-model="citationName" placeholder="Cite Original Website Name">
        <p></p>
        <input v-model="citationURL" placeholder="Cite Original Website URL">
        <p></p>
        <div class="check-box-list" v-if="tagSuggestions.length > 0">
          <div class="check-box-item" v-for="tag in tagSuggestions" :key="tag.id">
            <input class="checkbox" type="checkbox" :id="concatenate(addPrefix, tag.name)">
            <label :for="concatenate(addPrefix, tag.name)"> {{tag.name}}</label>
          </div>
        </div>
        <button @click="upload">Upload</button>
      </div>
      <div class="upload" v-if="addRecipe">
        <h2>{{addRecipe.title}}</h2>
        <img :src="addRecipe.path" />
        <p>{{addRecipe.description}}</p>
      </div>
    </div>

    <div class="heading">
      <h2>Edit/Delete a Recipe</h2>
    </div>
    <div class="edit">
      <div class="form">
        <input v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectRecipe(s)">{{s.title}}
          </div>
        </div>
      </div>

      <div class="upload" v-if="findRecipe">
        <input v-model="findRecipe.title" placeholder="Title">
        <p></p>
        <input v-model="findRecipe.ingredientsHeader1" placeholder="Ingredients Header 1">
        <p></p>
        <textarea v-model="findRecipe.ingredientsList1" placeholder="Ingredients List 1"></textarea>
        <p></p>
        <input v-model="findRecipe.ingredientsHeader2" placeholder="Ingredients Header 2">
        <p></p>
        <textarea v-model="findRecipe.ingredientsList2" placeholder="Ingredients List 2"></textarea>
        <p></p>
        <textarea v-model="findRecipe.instructionList" placeholder="Instruction List"></textarea>
        <p></p>
        <input v-model="findRecipe.citationName" placeholder="Original Website Name">
        <p></p>
        <input v-model="findRecipe.citationURL" placeholder="Original Website URL">
        <p></p>
        <div class="check-box-list" v-if="tagSuggestions.length > 0">
          <div class="check-box-item" v-for="tag in tagSuggestions" :key="tag.id">
            <div v-if="findRecipe.tags.includes(tag.name)">
              <input class="checkbox" type="checkbox" :id="concatenate(editPrefix, tag.name)" checked="true">
            </div>
            <div v-else>
              <input class="checkbox" type="checkbox" :id="concatenate(editPrefix, tag.name)">
            </div>
            <label :for="concatenate(editPrefix, tag.name)"> {{tag.name}}</label>
          </div>
        </div>
        <p></p>
        <img :src="findRecipe.path" />
      </div>
      <div class="actions" v-if="findRecipe">
        <button @click="deleteRecipe(findRecipe)">Delete</button>
        <button @click="editRecipe(findRecipe)">Save</button>
      </div>
    </div>

    <div class="heading">
      <h2>Add/Delete a Tag</h2>
    </div>
    <div class="tags">
      <div class="form">
        <input class="tag-input" v-model="tagName" placeholder="Click to add a Tag">
        <button @click="addTag">Add</button>
      </div>

      <div class="form">
        <input v-model="findName" placeholder="Search">
        <div class="suggestions" v-if="tagSuggestions.length > 0">
          <div class="suggestion" v-for="tagSug in tagSuggestions" :key="tagSug.id" @click="selectTag(tagSug)">{{tagSug.name}}
          </div>
        </div>
      </div>

      <div v-if="this.findTag != null">
        <h3>{{this.findTag.name}}</h3>
        <button @click="deleteTag(findTag)">Delete</button>
      </div>
    </div>
  </div>
  <div v-else>
    <h1>You must be an admin to access this page.</h1>
  </div>
</div>
</template>

<style scoped>

  .title-input, .header {
    font-weight: bold;
    text-align: center;
    color: #041E42;
    margin-bottom: 10px;
  }

  .title-input::placeholder {
    color: #041E42;
  }

  .ingredients-input {
    display: flex;
    text-wrap: wrap;
    margin-bottom: 10px;
  }

  .ingredient-sublist-input {
    margin: 10px;
  }

  .ingredient-sublist-header-input {
    padding-left: 5px;
  }

  .ingredient-sublist-content-input {
    padding-left: 5px;
    padding-top: 5px;
    min-height: 50px;
  }

  .header {
    text-align: left;
    padding-top: 10px;
  }

  .instructions-input {
    width: 100%;
    min-height: 75px;
    padding-left: 5px;
    padding-top: 5px;
  }

  button {
    background-color: #041E42;
    color: white;
    border-radius: 3px;
    padding: 5px;
    margin-right: 5px;
  }

 .check-box-list,
 .check-box-item {
  display: flex;
  flex-wrap: wrap;

 }

 .check-box-item {
  line-height: 40px;
  margin-right: 20px;
 }

 .checkbox {
  width: 20px;
  margin-right: 10px;
 }



  .admin {
    margin-left: 40px;
    margin-right: 40px;
    margin-bottom: 40px;
  }

  .image h2 {
    font-style: italic;
    font-size: 1em;
  }

  .heading {
    display: flex;
    margin-bottom: 20px;
    margin-top: 20px;
  }

  .heading h2 {
    margin-top: 8px;
    margin-left: 10px;
  }

  .add,
  .edit,
  .tags {
    display: flex;
  }

  .circle {
    border-radius: 50%;
    width: 18px;
    height: 18px;
    padding: 8px;
    background: #333;
    color: #fff;
    text-align: center
  }

  /* Form */
  input,
  textarea,
  select,
  button {
    font-family: 'Montserrat', sans-serif;
    font-size: 1em;
  }

  .form {
    margin-right: 50px;
  }

  /* Uploaded images */
  .upload h2 {
    margin: 0px;
  }

  .upload img {
    max-width: 300px;
  }

  /* Suggestions */
  .suggestions {
    width: 200px;
    border: 1px solid #ccc;
  }

  .suggestion {
    min-height: 20px;
  }

  .suggestion:hover {
    background-color: #5BDEFF;
    color: #fff;
  }
</style>

<script>
  import axios from 'axios';
  export default {
    name: 'Admin',
    data() {
      return {
        title: "",
        ingredientsHeader1: "",
        ingredientsList1: "",
        ingredientsHeader2: "",
        ingredientsList2: "",
        instructionList: "",
        citationName: "",
        citationURL: "",
        file: null,
        addRecipe: null,
        recipes: [],
        findTitle: "",
        findRecipe: null,
        tagName: "",
        tagList: [],
        findName: "",
        findTag: null,
        addPrefix: "add-",
        editPrefix: "edit-",
      }
    },
    computed: {
      suggestions() {
        let recipes = this.recipes.filter(recipe => recipe.title.toLowerCase().trim().includes(this.findTitle.toLowerCase().trim()));
        return recipes.sort((a, b) => (a.title.toLowerCase().trim() > b.title.toLowerCase().trim()) ? 1 : -1);
      },
      tagSuggestions() {
        let tags = this.tagList.filter(tag => tag.name.toLowerCase().trim().includes(this.findName.toLowerCase().trim()));
        return tags.sort((a, b) => (a.name.toLowerCase().trim() > b.name.toLowerCase().trim()) ? 1 : -1);
      }
    },
    created() {
      this.getRecipes();
      this.getTags();
    },
    methods: {
      fileChanged(event) {
        this.file = event.target.files[0]
      },
      clearRecipeInputs(){
        this.title = "";
        this.$refs.imageUpload.value = null;
        this.ingredientsHeader1 = "";
        this.ingredientsHeader2 = "";
        this.ingredientsList1 = "";
        this.ingredientsList2 = "";
        this.instructionList = "";
        this.citationName = "";
        this.citationURL = "";

        for (var tag of this.tagList) {
          document.getElementById(this.addPrefix + tag.name).checked = false;
        }
      },
      async upload() {
        try {
          const formData = new FormData();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/recipes', {
            title: this.title,
            path: r1.data.path,
            ingredientsHeader1: this.ingredientsHeader1,
            ingredientsList1: this.ingredientsList1,
            ingredientsHeader2: this.ingredientsHeader2,
            ingredientsList2: this.ingredientsList2,
            instructionList: this.instructionList,
            citationName: this.citationName,
            citationURL: this.citationURL,
            trueTags: this.getTrueTagChecboxNames(this.addPrefix)
          });
          this.addRecipe = r2.data;
          this.getRecipes();
          this.clearRecipeInputs();
        } catch (error) {
          console.log(error);
        }
      },
      async getRecipes() {
        try {
          let response = await axios.get("/api/recipes");
          this.recipes = response.data;
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      selectRecipe(recipe) {
        this.findTitle = "";
        this.findRecipe = recipe;
      },
      async deleteRecipe(recipe) {
        try {
          await axios.delete("/api/recipe/" + recipe._id);
          this.findRecipe = null;
          this.getRecipes();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      async editRecipe(recipe) {
        try {
          await axios.put("/api/recipe/" + recipe._id, {
            title: this.findRecipe.title,
            path: this.findRecipe.path,
            ingredientsHeader1: this.findRecipe.ingredientsHeader1,
            ingredientsList1: this.findRecipe.ingredientsList1,
            ingredientsHeader2: this.findRecipe.ingredientsHeader2,
            ingredientsList2: this.findRecipe.ingredientsList2,
            instructionList: this.findRecipe.instructionList,
            citationName: this.findRecipe.citationName,
            citationURL: this.findRecipe.citationURL,
            trueTags: this.getTrueTagChecboxNames(this.editPrefix)
          });
          this.findRecipe = null;
          this.getRecipes();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      async addTag() {
        if (this.tagName.trim() == "") {
          this.tagName = "";
          return;
        }

        let currentList = this.tagList.filter(tag => tag.name.trim() == this.tagName.trim());

        if (currentList.length != 0) {
          //console.log(currentList);
          //console.log(this.tagList.map(tag => tag.name));
          return;
        }

        try {
          await axios.post('/api/tags', {
            name: this.tagName.trim(),
          });
          this.tagName = "";
          this.getTags();
        } catch (error) {
          console.log(error);
        }
      },
      async getTags() {
        try {
          let response = await axios.get("/api/tags");
          this.tagList = response.data;
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      selectTag(tag) {
        this.findName = "";
        this.findTag = tag;
      },
      async deleteTag(tag) {
        try {
          await axios.delete("/api/tag/" + tag._id);
          this.findTag = null;
          this.getTags();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      concatenate(beginning, end) {
        return beginning + end;
      },
      getTrueTagChecboxNames(idPrefix) {
        let names = [];

        for (let theTag of this.tagList) {
          if(document.getElementById(idPrefix + theTag.name).checked) {
            names.push(theTag.name);
          }
        }
        return names;
      }
      /* TO DO:
        -add filter for tags
      */
    }
  }
</script>
