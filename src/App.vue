<script setup>
import { ref } from 'vue'

const error = ref(false)
const errors = ref()
const address = ref()
const data = ref({})

const iplookup = async () => {
  if (address.value.length === 0) {
    error.value = true
    errors.value = "Please enter a valid IP address"
    return  
  }

  let payload = {
    method: 'GET',
    mode: 'cors',
    headers: {
      'Authorization':'batman',
      'Account-ID':'batman',
      'Access-Control-Allow-Origin':'*'
    }
  }

  let url = `https://ipapi-image-rkl4iox4za-uw.a.run.app/${address.value}`
  await fetch(url, payload).then(async (res) => {
    if (!res.ok) {
      error.value = true
      errors.value = "Invalid IP address"
    } else {
      data.value = await res.json()
    }
  })
}

const reset = () => {
  error.value = false
  errors.value = ''
}

const hasData = () => Object.keys(data.value).length > 0 ? true : false

</script>

<template>
  <div class="navbar bg-primary">
    <a class="btn btn-ghost text-black text-xl font-bold">IP LookrUppr</a>
  </div>
  <div class="flex flex-col w-2/3 mx-auto p-4 bg-base-500 shadow lg:mt-10 lg:justify-center">
    <div class="flex flex-col mb-4">
      <label class="input input-bordered flex items-center gap-2">
        <input v-model="address" @change="iplookup" @blur="reset" type="text" class="grow" placeholder="IP address" />
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" fill="currentColor" class="w-4 h-4 opacity-70"><path fill-rule="evenodd" d="M9.965 11.026a5 5 0 1 1 1.06-1.06l2.755 2.754a.75.75 0 1 1-1.06 1.06l-2.755-2.754ZM10.5 7a3.5 3.5 0 1 1-7 0 3.5 3.5 0 0 1 7 0Z" clip-rule="evenodd" /></svg>
      </label>
    </div>
    <div v-if="error">
      {{ errors }} {{ address }}
    </div>
    <div v-if="hasData() === true">
      <div class="overflow-x-auto">
        <table class="table">
          <thead>
            <tr>
              <th>IP</th>
              <th>Country</th>
              <th>Continent</th>
              <th>ASN</th>
              <th>Domain</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <th>{{ data.ip }}</th>
              <th>{{ data.country }} ({{ data.country_code }})</th>
              <th>{{ data.continent_name }} ({{ data.continent }})</th>
              <th>{{ data.asn }}</th>
              <th>{{ data.as_domain }}</th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
</div>
</template>