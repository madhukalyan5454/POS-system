# POS-system
private boolean fetch = true; // we start to fetch right away
void start() {
    ScheduledExecutorService es = Executors.newScheduledThreadPool(1);
    es.scheduleAtFixedRate(this::scheduleFetch, FETCH_INTERVAL, FETCH_INTERVAL, TimeUnit.MILLISECONDS);
}
synchronized void scheduleFetch() {
    fetch = true;
    notify();
}
synchronized boolean awaitFetch() throws InterruptedException {
    if (!fetch)
        wait(WAIT_LIMIT);
    try {
        return fetch;
    } finally {
        fetch = false;
    }
}
List poll() throws InterruptedException {
    return awaitFetch() ? doFetch() : null;
}
