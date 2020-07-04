<template>
  <div>
    <h3>Dashboard</h3>
  </div>
</template>

<script lang="ts">
import { of } from 'rxjs'
import { fromFetch } from 'rxjs/fetch'
import { switchMap, catchError } from 'rxjs/operators'

export default {
  name: 'Dashboard',
  setup() {
    const data$ = fromFetch('https://api.github.com/users?per_page=5').pipe(
      switchMap(response => {
        if (response.ok) {
          // OK return data
          return response.json()
        } else {
          // Server is returning a status requiring the client to try something else.
          return of({ error: true, message: `Error ${response.status}` })
        }
      }),
      catchError(err => {
        // Network or other error, handle appropriately
        console.error(err)
        return of({ error: true, message: err.message })
      })
    )

    data$.subscribe({
      next: result => console.log(result),
      complete: () => console.log('done'),
    })
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
