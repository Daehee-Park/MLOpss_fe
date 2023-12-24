<div class="spinner" />
<span class="text-gray-500">Uploading...</span>

<style>
    .spinner {
        border: 4px solid rgba(0, 0, 0, 0.1);
        width: 40px;
        height: 40px;
        border-radius: 50%;
        @apply border-s-teal-600;
        animation: spin 0.8s ease-in-out infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>