    <script setup>
    import { ref } from 'vue'

    const props = defineProps({
    post: { type: Object, required: true },
    })

    // Local reactions state (so it won't affect PostCard.vue)
    const reactionTypes = [
    { type: 'like', label: '👍', color: 'text-blue-600' },
    { type: 'love', label: '❤️', color: 'text-red-500' },
    { type: 'haha', label: '😂', color: 'text-yellow-500' },
    { type: 'wow', label: '😮', color: 'text-purple-500' },
    { type: 'sad', label: '😢', color: 'text-blue-400' },
    { type: 'angry', label: '😡', color: 'text-red-600' },
    ]

    const userReaction = ref(null)
    const reactions = ref(
    props.post.reactions || {
        like: 0,
        love: 0,
        haha: 0,
        wow: 0,
        sad: 0,
        angry: 0,
    },
    )

    function setReaction(type) {
    if (userReaction.value === type) {
        reactions.value[type]--
        userReaction.value = null
    } else {
        if (userReaction.value) reactions.value[userReaction.value]--
        reactions.value[type]++
        userReaction.value = type
    }
    }
    </script>

    <template>
    <div class="mt-3 flex items-center space-x-6 text-gray-600">
        <!-- Reaction -->
        <div class="relative group">
        <button class="flex items-center hover:text-indigo-600">
            <span v-if="userReaction" class="mr-1">
            {{ reactionTypes.find((r) => r.type === userReaction)?.label }}
            </span>
            <span>{{ userReaction || 'React' }}</span>
            <span class="ml-1">({{ Object.values(reactions).reduce((a, b) => a + b, 0) }})</span>
        </button>

        <!-- Picker -->
        <div
            class="absolute bottom-full mb-2 left-0 bg-white shadow-lg rounded-full px-3 py-2 flex space-x-2 opacity-0 group-hover:opacity-100 transition"
        >
            <button
            v-for="r in reactionTypes"
            :key="r.type"
            @click="setReaction(r.type)"
            class="text-xl hover:scale-125 transition"
            :class="r.color"
            >
            {{ r.label }}
            </button>
        </div>
        </div>

        <!-- Comment -->
        <button class="flex items-center hover:text-indigo-600">
        💬 Comment ({{ post.comments?.length || 0 }})
        </button>

        <!-- Share -->
        <button class="flex items-center hover:text-indigo-600">
        🔗 Share
        </button>
    </div>
    </template>
