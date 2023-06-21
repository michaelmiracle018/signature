<template>
    <div
        v-if="loaded"
        class="d-flex justify-content-center py-4 mt-3 main__container"
    >
      <!-- <Counter
      :year="2023"
      :month="3"
      :date="28"
      :hour="23"
      :minute="59"
      :second="59"
      :millisecond="31"
    /> -->
        <div>
            <div class="text__expired">
                <h5 v-if="expiredLicense">License is expired</h5>
            </div>
            <div>
                <span class="time">{{ displayDays }}</span>
                <span class="time">:</span>
                <span class="time">{{ displayHours }}</span>
                <span class="time">:</span>
                <span class="time">{{ displayMinutes }}</span>
                <span class="time">:</span>
                <span class="time">{{ displaySeconds }}</span>

                <!-- <div class="day-text">
        <span>days</span>
        <span>hours</span>
        <span>minutes</span>
        <span>seconds</span>
      </div> -->
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['year', 'month', 'day', 'hour', 'minute', 'second', 'millisecond'],
    data() {
        return {
            displayDays: 0,
            displayHours: 0,
            displayMinutes: 0,
            displaySeconds: 0,
            loaded: false,
            expiredLicense: false,
        }
    },
    mounted() {
        this.showRemaining()
    },
    computed: {
        _seconds: () => 1000,
        _minutes() {
            return this._seconds * 60
        },
        _hours() {
            return this._minutes * 60
        },
        _days() {
            return this._hours * 24
        },
        end() {
            return new Date(
                this.year,
                this.month,
                this.day,
                this.hour,
                this.second,
                this.millisecond,
            )
        },
        // expiredLicenseFunc:() => this.expiredLicense
    },
    methods: {
        formatNum(num) {
            return num < 10 ? '0' + num : num
        },
        showRemaining() {
            const timer = setInterval(() => {
                const now = new Date()
                // const end = new Date(2023, 2, 30, 10, 10, 10, 10)
                const distance = this.end.getTime() - now.getTime()

                if (distance < 0) {
                    clearInterval(timer)
                    this.loaded = true
                    this.expiredLicense = true
                    return
                }

                const days = Math.floor(distance / this._days)
                const hours = Math.floor((distance % this._days) / this._hours)
                const minutes = Math.floor(
                    (distance % this._hours) / this._minutes,
                )
                const seconds = Math.floor(
                    (distance % this._minutes) / this._seconds,
                )

                this.displayMinutes = this.formatNum(minutes)
                this.displaySeconds = this.formatNum(seconds)
                this.displayHours = this.formatNum(hours)
                this.displayDays = this.formatNum(days)
                this.loaded = true
            }, 1000)
        },
    },
}
</script>

<style scoped>
.main__container .time {
    font-size: 18px;
}

.day-text {
    padding-left: 10px;
}

.day-text span {
    margin-right: 35px;
    font-size: 12px;
}

.text__expired h5 {
    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 20px;
    line-height: 24px;
    color: #000000;
}
</style>
