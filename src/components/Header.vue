<template>
    <div>
    <b-nav tabs>
        <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
        <b-nav-item disabled>Counter: {{correctAnswer}} /{{totalAnswer}}</b-nav-item>
    </b-nav>
    </div>
</template>

<script>
export default {
    props: {
        correctAnswer: Number,
        totalAnswer: Number
    }
}
</script>