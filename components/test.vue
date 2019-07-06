<template>
    <div>hello world</div>
</template>

<script>

export default {
  components: {
    
  },
  data: () => {
    return {
      items: [
        {
          title: "Admin",
          to: "/admin"
        },
        {
          title: "Wizard",
          to: "/wizard"
        }
      ]
    }
  }
}
</script>
