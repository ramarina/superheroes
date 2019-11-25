<script>
import firebase from 'firebase';

export default {
    created() {
        if(firebase.auth().currentUser){
            this.$router.push({
                path: 'home'
            })
        }
        this.$router.push({
            path: 'login'
        })
    }
}
</script>
