import React, { useMemo, useState } from "react";

const defaultData = {
  overallScore: 58,
  varcAccuracy: 72,
  dilrAccuracy: 38,
  qaAccuracy: 51,
  varcTime: 42,
  dilrTime: 38,
  qaTime: 40,
  weakTopics: "DILR set selection, Arithmetic, Algebra",
};

function getInsights(data) {
  const issues = [];
  const plan = [];

  if (data.dilrAccuracy < 45) {
    issues.push("DILR set selection is a major score blocker.");
    plan.push("Day 1: Solve 4 previous DILR sets with a 10-minute set selection drill.");
  }
  if (data.qaAccuracy < 60) {
    issues.push("Quant accuracy is too low for the time invested.");
    plan.push("Day 2: Revise Arithmetic and Algebra basics, then attempt 20 medium questions.");
  }
  if (data.varcAccuracy > 70 && data.varcTime > 40) {
    issues.push("VARC is relatively strong but may be taking too much time.");
    plan.push("Day 3: Attempt one VARC sectional with stricter pacing.");
  }
  if (issues.length === 0) {
    issues.push("No major red flags. Focus on consistency and revision.");
    plan.push("Day 1-3: Maintain balanced sectionals and review error log.");
  }

  return {
    issues,
    plan,
    summary:
      "Your score can improve fastest by fixing the weakest decision pattern, not by studying everything equally.",
    confidence:
      data.overallScore < 60
        ? "This mock looks recoverable. Your biggest gain will come from sharper prioritization, not more hours."
        : "You already have a decent base. Targeted correction can create a visible percentile jump.",
  };
}

export default function MockToPlanPrototype() {
  const [form, setForm] = useState(defaultData);
  const insights = useMemo(() => getInsights(form), [form]);

  const update = (key, value) => setForm((p) => ({ ...p, [key]: Number(value) || value }));

  return (
    <div style={{ maxWidth: 900, margin: "0 auto", padding: 24, fontFamily: "Inter, sans-serif" }}>
      <h1>Mock-to-Plan</h1>
      <p>Convert one CAT mock into a focused 3-day action plan.</p>

      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 16, marginTop: 20 }}>
        <input placeholder="Overall Score" value={form.overallScore} onChange={(e) => update("overallScore", e.target.value)} />
        <input placeholder="VARC Accuracy %" value={form.varcAccuracy} onChange={(e) => update("varcAccuracy", e.target.value)} />
        <input placeholder="DILR Accuracy %" value={form.dilrAccuracy} onChange={(e) => update("dilrAccuracy", e.target.value)} />
        <input placeholder="QA Accuracy %" value={form.qaAccuracy} onChange={(e) => update("qaAccuracy", e.target.value)} />
        <input placeholder="VARC Time" value={form.varcTime} onChange={(e) => update("varcTime", e.target.value)} />
        <input placeholder="DILR Time" value={form.dilrTime} onChange={(e) => update("dilrTime", e.target.value)} />
        <input placeholder="QA Time" value={form.qaTime} onChange={(e) => update("qaTime", e.target.value)} />
        <input placeholder="Weak Topics" value={form.weakTopics} onChange={(e) => update("weakTopics", e.target.value)} />
      </div>

      <div style={{ marginTop: 28, padding: 20, border: "1px solid #ddd", borderRadius: 12 }}>
        <h2>What hurt your score</h2>
        <ul>
          {insights.issues.map((item, i) => <li key={i}>{item}</li>)}
        </ul>

        <h2 style={{ marginTop: 20 }}>3-day action plan</h2>
        <ul>
          {insights.plan.map((item, i) => <li key={i}>{item}</li>)}
        </ul>

        <h2 style={{ marginTop: 20 }}>Summary</h2>
        <p>{insights.summary}</p>

        <h2 style={{ marginTop: 20 }}>Confidence note</h2>
        <p>{insights.confidence}</p>
      </div>
    </div>
  );
}
