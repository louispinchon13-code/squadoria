-- Table des demandes d'ami réelles
create table if not exists friend_requests (
  id uuid default gen_random_uuid() primary key,
  from_pseudo text,
  to_pseudo text,
  status text default 'en_attente',
  created_at timestamptz default now()
);

-- Table des groupes réels
create table if not exists groupes (
  id uuid default gen_random_uuid() primary key,
  nom text,
  createur text,
  membres int default 1,
  created_at timestamptz default now()
);

-- On autorise tout le monde à voir et créer, comme pour profils et messages
alter table friend_requests enable row level security;
alter table groupes enable row level security;

create policy "public read friend" on friend_requests for select using (true);
create policy "public insert friend" on friend_requests for insert with check (true);
create policy "public read groupes" on groupes for select using (true);
create policy "public insert groupes" on groupes for insert with check (true);
