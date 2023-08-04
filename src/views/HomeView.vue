<template>
  <div class="content">

    <!--Image & Title-->
    <img
      src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/39be584c-4045-48b4-b129-d0be45b1a81a/dfraxsm-12ee9efb-5bd3-41ab-939e-e9fc645a1ba0.png/v1/fill/w_894,h_894,q_70,strp/ichigo_by_trini3designs_dfraxsm-pre.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7ImhlaWdodCI6Ijw9MTAyNCIsInBhdGgiOiJcL2ZcLzM5YmU1ODRjLTQwNDUtNDhiNC1iMTI5LWQwYmU0NWIxYTgxYVwvZGZyYXhzbS0xMmVlOWVmYi01YmQzLTQxYWItOTM5ZS1lOWZjNjQ1YTFiYTAucG5nIiwid2lkdGgiOiI8PTEwMjQifV1dLCJhdWQiOlsidXJuOnNlcnZpY2U6aW1hZ2Uub3BlcmF0aW9ucyJdfQ.PFxvirZ9uK9qRptTAqFtcjeOsd3_iO46LJA5Qbw7U0w">
    <div class="text">Welcome to the loop</div>

    <form action="#">
      <!--Email & password input place-->
      <div class="field" v-for="input in inputs" :key="input.id">
        <span :class="input.icon"></span>
        <input type="text" :id="input.id" @focus="setFocusedInput(input.id)" />



      </div>

      <!--login & outer button-->
    </form>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';

interface Input {
  id: string;
  icon: string;
  label: string;
  focused: boolean;
}


const inputs = ref<Input[]>([
  { id: 'inputField1', icon: 'fa fa-user', label: 'Username', focused: false },
  { id: 'inputField2', icon: 'fa fa-lock', label: 'Password', focused: false },

]);

const handleMessageFromParent = (event: MessageEvent): void => {
  console.log({ event })
  let originSrc = event.origin // this value should be changed to your projects origin
  
  if (event.origin === originSrc) {
    console.log('Message received from parent:', event.data);

    if (event.data === 'backspace') {
      handleBackspace();
    } else {
      const focusedInput = inputs.value.find((input) => input.focused);
      if (focusedInput) {
        const inputElement = document.getElementById(focusedInput.id) as HTMLInputElement;
        if (inputElement) {
          inputElement.value += event.data; // Append the received character to the focused input field
          inputElement.focus(); // Keep the input field focused after receiving the message
        }
      }
    }
  }
};

const handleBackspace = (): void => {
  const focusedInput = inputs.value.find((input) => input.focused);
  if (focusedInput) {
    const inputElement = document.getElementById(focusedInput.id) as HTMLInputElement;
    if (inputElement) {
      const currentValue = inputElement.value;
      inputElement.value = currentValue.slice(0, -1); // Remove the last character from the input field
      inputElement.focus(); // Keep the input field focused after backspace
    }
  }
};

const setFocusedInput = (id: string): void => {
  inputs.value.forEach((input) => {
    input.focused = input.id === id;
  });
};

onMounted(() => {
  window.addEventListener('message', handleMessageFromParent);
});

onUnmounted(() => {
  window.removeEventListener('message', handleMessageFromParent);
});
</script>