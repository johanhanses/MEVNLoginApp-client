<template>
    <div>
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
            <div class="container">
                <a class="navbar-brand" href="/">Home</a>
                <button
                    class="navbar-toggler"
                    type="button"
                    data-toggle="collapse"
                    data-target="#navbarNav"
                    aria-controls="navbarNav"
                    aria-expanded="false"
                    aria-label="Toggle navigation"
                >
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div
                    class="collapse navbar-collapse justify-content-end"
                    id="navbarNav"
                >
                    <ul class="navbar-nav">
                        <li class="nav-item active">
                            <a
                                class="nav-link"
                                style="cursor: pointer;"
                                @click="logUserOut"
                            >
                                Logout</a
                            >
                        </li>
                    </ul>
                </div>
            </div>
        </nav>

        <section>
            <div class="container mt-5">
                <div class="row">
                    <div class="col-md-6 center">
                        <ul class="list-group">
                            <li class="list-group-item">
                                Name : {{ user.name }}
                            </li>
                            <li class="list-group-item">
                                Email : {{ user.email }}
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import VueJwtDecode from "vue-jwt-decode";

export default {
    data() {
        return {
            user: {},
        };
    },

    methods: {
        getUserDetails() {
            let token = localStorage.getItem("jwt");
            let decoded = VueJwtDecode.decode(token);
            this.user = decoded;
        },
        async logUserOut() {
            let token = localStorage.getItem("jwt");

            const headers = {
                "Content-Type": "application/json",
                Authorization: "Bearer " + token,
            };

            await this.$http.post("/user/logout", headers);

            localStorage.removeItem("jwt");
            this.$router.push("/login");
        },
    },

    created() {
        this.getUserDetails();
    },
};
</script>

<style scoped>
.center {
    margin: auto;
}
</style>
