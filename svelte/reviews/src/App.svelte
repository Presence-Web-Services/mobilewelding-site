<script>
let form, name, email, review, hp;
let stars = 5;
let nameError, emailError, starsError, reviewError, hpError;
let responseError, responseSuccess;
let sending;
$: disabled = nameError || emailError || starsError || reviewError || hpError || responseError || sending;

function checkName() {
  if (!name) {
    nameError = 'Error: Please enter your name.';
    return;
  }
  else if (name.length > 100) {
    nameError = 'Error: Your name is too long.';
    return;
  }
}

function checkEmail() {
  if (!email) {
    emailError = 'Error: Please enter an email.';
    return;
  }
  else if (email.length < 5 || email.length > 50) {
    emailError = 'Error: Email is too short or too long.';
    return;
  }
  let emailRegex = new RegExp("^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$");
  if (!emailRegex.test(email)) {
    emailError = 'Error: Email is not a valid format.';
    return;
  }
}

function checkStars() {
  console.dir(stars);
  if (!(stars >= 1 && stars <= 5)) {
    starsError = "Error: Star count not in range 1-5."
    return;
  }
}

function checkreview() {
  if (!review) {
    reviewError = 'Error: Please enter a review.';
    return;
  }
  else if (review.length > 2000) {
    reviewError = 'Error: review is too long.';
    return;
  }
}

function checkHP() {
  if (hp) {
    hpError = 'Error: Please, no robots!';
  }
}

function clearFields() {
  name = email = stars = review = hp = "";
}

function sendData() {
  const xhr = new XMLHttpRequest();
  const formData = new FormData(form);
  xhr.addEventListener('load', (event) => {
    if (event.target.status != 200) {
      responseError = event.target.responseText.trim();
    } else {
      responseSuccess = event.target.responseText.trim();
      clearFields();
    }
    sending = false;
  });
  xhr.addEventListener('error', (event) => {
    responseError = 'Error: No response from server.'
    sending = false;
  });
  xhr.open('POST', 'https://inquirer-c4kbbrskpq-uc.a.run.app');
  xhr.send(formData);
}

function sendEmail(event) {
  nameError = emailError = starsError = reviewError = hpError = responseError = responseSuccess = "";
  sending = true;
  checkName();
  checkEmail();
  checkStars();
  checkreview();
  checkHP();
  if (nameError || emailError || starsError || eviewError || hpError) {
    sending = false;
    return;
  }
  sendData();
}

function nameChange() {
  nameError = responseError = "";
}

function emailChange() {
  emailError = responseError = "";
}

function starChange() {
  starsError = responseError = "";
}

function reviewChange() {
  reviewError = responseError = "";
}

function hpChange() {
  hpError = responseError = "";
}

</script>

<form bind:this={form}>
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" size="33" maxlength="100" required bind:value={name} on:input={nameChange}>
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" size="33" maxlength="50" required bind:value={email} on:input={emailChange}>
  <section id="star-rating">
    <h4>Star Rating:</h4>
    <input type="radio" id="radio-1" name="stars" bind:group={stars} value={1} on:input={starChange}>
    <label for="radio-1">1</label>
    <input type="radio" id="radio-2" name="stars" bind:group={stars} value={2} on:input={starChange}>
    <label for="radio-2">2</label>
    <input type="radio" id="radio-3" name="stars" bind:group={stars} value={3} on:input={starChange}>
    <label for="radio-3">3</label>
    <input type="radio" id="radio-4" name="stars" bind:group={stars} value={4} on:input={starChange}>
    <label for="radio-4">4</label>
    <input type="radio" id="radio-5" name="stars" bind:group={stars} value={5} on:input={starChange}>
    <label for="radio-5">5</label>
  </section>
  <label for="review">Review:</label>
  <textarea id="review" name="review" rows="12" cols="33" maxlength="2000" required bind:value={review} on:input={reviewChange}></textarea>
  <input id="hp" name="hp" bind:value={hp} on:input={hpChange}>
  <button on:click|preventDefault={sendEmail} {disabled}>Send</button>
</form>
{#if nameError}
  <p aria-live="polite" class="error">{nameError}</p>
{/if}
{#if emailError}
  <p aria-live="polite" class="error">{emailError}</p>
{/if}
{#if starsError}
  <p aria-live="polite" class="error">{starsError}</p>
{/if}
{#if reviewError}
  <p aria-live="polite" class="error">{reviewError}</p>
{/if}
{#if hpError}
  <p aria-live="polite" class="error">{hpError}</p>
{/if}
{#if responseError}
  <p aria-live="polite" class="error">{responseError}</p>
{/if}
{#if responseSuccess}
  <p aria-live="polite" class="success">{responseSuccess}</p>
{/if}

<style>
form * {
  display: block;
}
form input, form textarea {
  font-size: 1em;
  font-family: monospace;
  max-width: calc(100% - 2em);
  width: 22rem;
}
#hp {
  visibility: hidden;
  height: 0;
  width: 0;
  margin: 0;
  padding: 0;
  border: 0;
}
p.error {
  color: red;
}
p.success {
  color: green;
}
#star-rating h4 {
  font-weight: normal;
  margin: 0;
}
#star-rating input, #star-rating label {
  display: inline-block;
  max-width: initial;
  width: initial;
}
</style>
