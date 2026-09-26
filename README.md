# My-second-project-
My first project while learning programming and GitHub.
export type Side = "BUY" | "SELL";

export type Signal = {
  id: string;
  kind: "Buy" | "Sell" | "Hold";
  text: string;
};

export const signals: Signal[] = [
  { id: "s1", kind: "Buy", text: "RSI reversal off 30 on 1H + bullish engulfing. Target 2,431." },
  { id: "s2", kind: "Sell", text: "Rejected at 2,425 resistance with lower high. Take partial profit." },
  { id: "s3", kind: "Hold", text: "ATR below threshold — no new entries, trailing stop active." },
];

export const openPositions = [
  { id: "p1", side: "BUY" as Side, lots: 0.85, entry: 2405.2, stop: 2398.8, pnl: 114.0 },
  { id: "p2", side: "SELL" as Side, lots: 0.4, entry: 2421.7, stop: 2429.1, pnl: -12.2 },
];

export const closedTrades = [
  { id: "c1", side: "BUY" as Side, label: "Buy 0.60 · TP 2,419.40", when: "Mon 14:20", pnl: 86.4 },
  { id: "c2", side: "SELL" as Side, label: "Sell 0.30 · SL 2,402.00", when: "Sun 09:05", pnl: -24.0 },
];

/** Deterministic seeded price path so server and client render the same first frame. */
export function buildSeries(points = 48, base = 2418.6) {
  const out: number[] = [];
  let v = base - 14;
  for (let i = 0; i < points; i++) {
    const wave = Math.sin(i / 5.5) * 3.4 + Math.sin(i / 2.1) * 1.3;
    v += 0.32 + wave * 0.28;
    out.push(v);
  }
  return out;
}

export function toPath(values: number[], width = 300, height = 96) {
  const min = Math.min(...values);
  const max = Math.max(...values);
  const span = max - min || 1;
  return values
    .map((v, i) => {
      const x = (i / (values.length - 1)) * width;
      const y = height - ((v - min) / span) * (height - 8) - 4;
      return `${i === 0 ? "M" : "L"}${x.toFixed(2)},${y.toFixed(2)}`;
    })
    .join(" ");
}

export const fmt = (n: number, d = 2) =>
  n.toLocaleString("en-US", { minimumFractionDigits: d, maximumFractionDigits: d });
Analysis trading charts
