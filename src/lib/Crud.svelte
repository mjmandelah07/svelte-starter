<script>
  let foods = [];
  let newItem = "";
  let editItem = null;
  let editedText = "";

  // A reactive statement to load foods from local storage
  $: {
    const storedFoods = localStorage.getItem("foods");
    if (storedFoods) {
      foods = JSON.parse(storedFoods);
    }
    console.log(foods);
  }

  const addItem = () => {
    if (newItem.trim() !== "") {
      // Check if a case-insensitive duplicate item exists in the array
      const duplicateItem = foods.find(
        (item) => item.text.toLowerCase() === newItem.toLowerCase()
      );

      if (!duplicateItem) {
        // Capitalize the first letter of newItem
        const newItemText = newItem.charAt(0).toUpperCase() + newItem.slice(1);
        const newItemObject = { id: Date.now(), text: newItemText };
        foods = [...foods, newItemObject];
        newItem = "";

        // Save items to local storage
        localStorage.setItem("foods", JSON.stringify(foods));
        alert("Food item is added");
      } else {
        alert("Food item already exists");
        newItem = "";
      }
    }
  };

  const editItemText = (/** @type {any} */ id) => {
    // find if the item exists
    const itemToEdit = foods.find((item) => item.id === id);
    // if item exists then assign the editItem with the data
    if (itemToEdit) {
      editItem = itemToEdit;
      editedText = itemToEdit.text;
    }
  };

  const updateItem = () => {
    if (editedText.trim() !== "") {
      // Capitalize the first letter of editedText
      const updatedText =
        editedText.charAt(0).toUpperCase() + editedText.slice(1);

      editItem.text = updatedText;
      editItem = null;
      editedText = "";

      // Save items to local storage after an update
      localStorage.setItem("foods", JSON.stringify(foods));
    }
  };

  /**
   * @param {any} id
   */
  function deleteItem(id) {
    foods = foods.filter((item) => item.id !== id);
    // checks if an item is currently being edited, if so, exits the editing mode and clear the editing text field.
    if (editItem && editItem.id === id) {
      editItem = null;
      editedText = "";
    }

    // Save items to local storage after a delete
    localStorage.setItem("foods", JSON.stringify(foods));
  }
</script>

<main>
  
  <input bind:value={newItem} id="newitem" placeholder="Add new item" />
  <button on:click={addItem}>Add Item</button>

  <div id="items">
    <table>
      <thead>
        <tr>
          <th>Food Item</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        {#each foods as food (food.id)}
          <tr>
            <td>
              {#if editItem === food}
                <input bind:value={editedText} />
              {:else}
                {food.text}
              {/if}
            </td>
            <td>
              {#if editItem === food}
                <button on:click={updateItem}>Update</button>
              {:else}
                <button on:click={() => editItemText(food.id)}>Edit</button>
              {/if}
              <button on:click={() => deleteItem(food.id)}>Delete</button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</main>

<style>
  button {
    margin-left: 20px;
    padding: 8px 12px;
    outline: none;
    border: none;
    border-radius: 5px;
    background-color: #1c5cd2;
    color: white;
    cursor: pointer;
  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    padding: 10px;
    border-bottom: 1px solid #ddd;
    text-align: left;
  }

  th {
    background-color: #115888;
  }

  input {
    padding: 5px;
  }

  #items {
    margin-top: 20px;
  }
</style>
