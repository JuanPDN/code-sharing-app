<template>
    <header>
        <img src="@/assets/NoteCodeLogo.svg" alt="NoteCode logo" class="logo" height="20" width="111">
        <div>
            <h1>Create & Share</h1>
            <h2>Your Code easily</h2>
        </div>
    </header>
</template>

<style scoped>
div {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    margin: 36px 0px;
}

header {
    display: flex;
    flex-direction: column;
    align-items: center;
}

h1 {
    text-align: center;
    font-size: 2rem;
    font-weight: 600;
}

h2 {
    text-align: center;
    font-size: 2.5rem;
    font-weight: 600;
}

.logo {
    margin-top: 40px;
}
</style>
