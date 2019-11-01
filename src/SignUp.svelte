<script>
  import { fade } from "svelte/transition";
  import Fieldset from "./Fieldset";

  let firstName = "";
  let lastName = "";
  let isError = false;
  let isSubmitted = false;
  let isPasswordError = false;

  const errors = {
    INVALID_EMAIL: "Please enter a valid email address.",
    PASSWORD_MISMATCH: "Your passwords do not match.",
    MISSING_FIELD: "You cannot leave this field blank."
  };

  const formatName = name => {
    return (name.charAt(0).toUpperCase() + name.slice(1)).trim();
  };

  const validateForm = () => {
    const email = document.getElementById("email").value;
    const password = document.getElementById("password").value;
    const confirmation = document.getElementById("confirmation").value;

    if (firstName && lastName && email && !isPasswordError) {
      isSubmitted = true;
    }
  };

  const validatePassword = event => {
    const password = document.getElementById("password").value;
    if (event.target.value !== password) {
      isPasswordError = true;
    } else {
      isPasswordError = false;
    }
  };

  const setFirstName = event => {
    firstName = formatName(event.target.value);
  };

  const setLastName = event => {
    lastName = formatName(event.target.value);
  };
</script>

<div class="measure center">
  <h1>Sign Up</h1>
  <p>Register an account to help us wool the world.</p><p class="b pb3">All fields are required.</p>
  <form on:submit|preventDefault={validateForm} on:invalid={validateForm}>
    <Fieldset legend="What's your name?">
      <div class="mt3">
        <label class="db fw6 lh-copy f6" for="first-name">First Name</label>
        <input
          on:input={setFirstName}
          class="pa2 input-reset ba bg-transparent w-100"
          type="text"
          name="first-name"
          id="first-name"
          required
          disabled={isSubmitted} />
      </div>
      <div class="mt3">
        <label class="db fw6 lh-copy f6" for="last-name">Last Name</label>
        <input
          on:input={setLastName}
          class="pa2 input-reset ba bg-transparent w-100"
          type="text"
          name="last-name"
          id="last-name"
          required
          disabled={isSubmitted} />
      </div>
    </Fieldset>
    {#if firstName && lastName}
      <h2 transition:fade>Hi, {firstName} {lastName}!</h2>
      <div in:fade={{ delay: 350, duration: 1000 }}>
        <Fieldset
          legend="A few more steps, and ewe can be one of us.">
          <div class="mt3">
            <label class="db fw6 lh-copy f6" for="email">Email</label>
            <input
              class="pa2 input-reset ba bg-transparent w-100"
              type="email"
              name="email"
              id="email"
              required
              disabled={isSubmitted} />
          </div>
          <div class="mv3">
            <label class="db fw6 lh-copy f6" for="password">Password</label>
            <input
              class="b pa2 input-reset ba bg-transparent w-100"
              type="password"
              name="password"
              id="password"
              required
              disabled={isSubmitted} />
          </div>
          <div class="mv3">
            <label class="db fw6 lh-copy f6" for="confirmation">
              Confirm Password
            </label>
            <input
              on:input={validatePassword}
              class="b pa2 input-reset ba bg-transparent w-100"
              type="password"
              name="confirmation"
              id="confirmation"
              required
              disabled={isSubmitted} />
          </div>
          <p class="{isPasswordError ? '' : 'dn'} red">Your passwords do not match.</p>
        </Fieldset>
      </div>
      {#if !isSubmitted}
        <button
          class="b br2 ph3 pv2 input-reset ba b--black bg-transparent grow
          pointer f6 dib"
          type="submit">
          Join the Herd
        </button>
      {:else if !isError && isSubmitted}
        Nice job, {firstName} {lastName}! Your account details are now saved.
      {/if}
    {/if}
  </form>
</div>
